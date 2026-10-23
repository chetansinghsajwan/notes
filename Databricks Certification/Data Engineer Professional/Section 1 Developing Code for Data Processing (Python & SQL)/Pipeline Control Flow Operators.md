## Topic Overview
Lakeflow Jobs support control flow operators that let you build complex, dynamic workflows. Instead of always running tasks sequentially, you can branch logic based on task outcomes, loop over datasets, or dynamically generate tasks. These operators enable conditional execution, parameterized workflows, and smart retry logic without requiring custom orchestration code.
The main operators are if/else tasks for branching, for_each for looping over array parameters, and task values for passing data between tasks. When combined, they let you create self healing pipelines that skip unnecessary work or retry failed stages. Task outcomes (success, failure, skipped) drive these decisions.
On the exam, you will see questions about when to use control flow vs fixed task dependencies, how to pass parameters between tasks, and how to handle task failures elegantly. Understanding these patterns helps you design pipelines that adapt to data and runtime conditions.
---
## Key Concepts
**If/Else Task Branching**
If/else tasks conditionally execute other tasks based on a boolean condition. The condition can be a static parameter, a task outcome, or a metric from a previous task. For example, if a data quality check passes, proceed with transformation; otherwise, route to quarantine.
**For_Each Loop Operator**
For_each tasks iterate over an array parameter, creating a task instance for each element. Useful for processing multiple tenants, dates, or regions in parallel. All iterations run concurrently unless you set max parallelism.
**Task Values for Inter task Data Passing**
Tasks can set key value pairs via dbutils.jobs.taskValues.set(). Downstream tasks read them with dbutils.jobs.taskValues.get(). This enables parameterized pipelines where one task computes a value (e.g., row count) and the next task uses it (e.g., to decide if an aggregation is large enough to repartition).
**Task Outcomes and Conditional Logic**
Every task has an outcome: success, failure, or skipped. You can branch on outcome using if/else: if previous task succeeded, do X; if failed, do Y. This enables retry pipelines without manual intervention.
---
## Code Examples
### If/Else Task Configuration in YAML
```yaml
jobs:
  etl_pipeline:
    tasks:
      - task_key: validate_data
        notebook_task:
          notebook_path: /notebooks/validate
      - task_key: route_data
        condition_task:
          left: "{{tasks.validate_data.values.quality_score}}"
          op: ">"
          right: "0.95"
        depends_on:
          - task_key: validate_data
      - task_key: process_good
        depends_on:
          - task_key: route_data
        condition: "{{tasks.route_data.state == 'SUCCESS'}}"
        notebook_task:
          notebook_path: /notebooks/process
      - task_key: quarantine_bad
        depends_on:
          - task_key: route_data
        condition: "{{tasks.route_data.state == 'FAILED'}}"
        notebook_task:
          notebook_path: /notebooks/quarantine
```
The condition_task routes to process_good if quality_score > 0.95, otherwise to quarantine_bad. Each downstream task evaluates its condition property independently.
### Task Values: Set and Get Pattern
```python
# Task 1: compute_metrics
import pandas as pd
from pyspark.sql import functions as F
df = spark.table("my_catalog.my_schema.data")
row_count = df.count()
df_size_gb = spark.sql(f"SELECT SUM(size) / 1e9 AS size_gb FROM INFORMATION_SCHEMA.PARTITIONS WHERE table_schema = 'my_schema' AND table_name = 'data'").collect()[0][0]
dbutils.jobs.taskValues.set({"row_count": row_count, "size_gb": df_size_gb})
# Task 2: process_with_metrics
task_values = dbutils.jobs.taskValues.get(task_key="compute_metrics")
row_count = task_values["row_count"]
size_gb = task_values["size_gb"]
if row_count > 1_000_000 and size_gb > 10:
    # Use aggressive partitioning for large datasets
    spark.sql(f"ALTER TABLE my_table SET TBLPROPERTIES ('delta.dataSkippingNumIndexedCols' = '32')")
else:
    # Standard processing
    spark.sql(f"ALTER TABLE my_table SET TBLPROPERTIES ('delta.dataSkippingNumIndexedCols' = '10')")
```
Task 1 computes metrics and stores them as task values. Task 2 retrieves them and adapts processing based on data size. This pattern enables self tuning pipelines.
### For Each Loop Configuration
```yaml
jobs:
  etl_pipeline:
    tasks:
      - task_key: process_regions
        for_each_task:
          list: "{{job.parameters.regions}}"
          variable: "region"
        notebook_task:
          notebook_path: /notebooks/process_region
          base_parameters:
            region: "{{region}}"
        new_cluster:
          spark_version: "14.3.x-scala2.12"
          node_type_id: "m6i.xlarge"
          num_workers: 2
```
If regions = ['US', 'EU', 'APAC'], Lakeflow creates three task instances automatically, one for each region. All run in parallel (unless you set max_parallelism). Each receives the region value as a parameter.
---
## Common Exam Scenarios
**Scenario 1: Multi tenant data pipeline with dynamic routing**
You have 100 customer tenants and want to process each in parallel. Use for_each to loop over the tenant list and create a task instance for each. Each tenant's data is isolated and processed independently. If one fails, others continue. This avoids hardcoding 100 separate task definitions.
**Scenario 2: Adaptive pipeline based on data quality**
A data validation task scores data quality (0-100). If score > 90, proceed to production load. If 70-90, route to a cleansing step. If < 70, quarantine and alert. Use condition_task and task values to set thresholds dynamically. Each branch knows what to do based on actual data quality, not static rules.
**Scenario 3: Cost optimization with dynamic cluster sizing**
The first task profiles data size. If > 100 GB, use large cluster for transformation. If < 100 GB, use small cluster. Store the cluster size as a task value and pass it to downstream tasks as a parameter. This optimizes cost and performance without manual intervention.
---
## Key Takeaways
- If/else tasks and condition_task enable conditional branching based on metrics or task outcomes
- For_each loops dynamically create task instances for each element in a list, enabling parallel processing of multiple tenants or date ranges
- Task values (dbutils.jobs.taskValues.set/get) pass data between tasks without external state stores
- Control flow operators reduce boilerplate: use them instead of hardcoding many similar tasks
- Combine control flow with task values for self healing pipelines that adapt to data and runtime conditions
---
## Gotchas and Tips
⚠️ Condition must resolve correctly: If a condition references a task value or task state that does not exist, the task is skipped. Always test conditions with actual task outputs.
⚠️ For_each creates new clusters for each iteration by default: Set resource constraints or use existing_cluster_id to reuse clusters and save cost.
⚠️ Task values are strings: When you retrieve a task value, it is a string. Parse to int/float if needed. dbutils.jobs.taskValues.get() returns a dict.
---
## Links and Resources
- **[Lakeflow SDP Concepts](https://docs.databricks.com/aws/en/ldp/concepts)** — Understanding pipeline control flow, expectations, and dataset dependencies.
- **[Lakeflow Spark Declarative Pipelines](https://docs.databricks.com/aws/en/ldp/)** — Full reference for building and managing declarative data pipelines.