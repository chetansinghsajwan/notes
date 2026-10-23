## Topic Overview
Lakeflow Jobs (formerly called Databricks Workflows) are the central orchestration mechanism for scheduling and executing data pipelines on Databricks. When you have built your pipeline logic across multiple notebooks or scripts, you need a way to run them reliably on a schedule, handle failures gracefully, and monitor their execution. That's what jobs do.
A job is a collection of one or more tasks arranged in a directed acyclic graph (DAG). Each task can run on different clusters, use different compute resources, and depend on the success or failure of other tasks. This flexibility lets you build sophisticated multi step pipelines: ingest data, transform it, run quality checks, and load results to a warehouse all in one job with clear dependency management.
One of the most practical features is the ability to repair and rerun failed runs. If a job fails partway through, you don't have to restart from the beginning. You can fix the underlying issue (bad data, downstream API timeout, etc.) and rerun only the failed tasks and anything that depends on them. This is critical in production environments where you want to minimize wasted compute and keep pipelines responsive to issues.
---
## Key Concepts
Lakeflow Jobs Overview
Lakeflow Jobs are Databricks managed orchestration. You define a job via UI, API, or YAML (in Databricks Asset Bundles). The Databricks platform handles scheduling, retrying, monitoring, and cluster provisioning. Each job has a unique ID and can be triggered on a schedule (cron), manually, by file arrival, or continuously. Jobs are the standard way to operationalize data pipelines in production.
Task Types and DAG
A job consists of one or more tasks. Supported task types include: Notebook, Python, JAR, SQL, Lakeflow Pipeline, dbt, and Spark Submit. Tasks form a DAG where you define dependencies: "run task B only after task A succeeds". This allows parallel execution of independent tasks and strict sequencing of dependent ones.
Scheduling Options
Jobs can be triggered in multiple ways: cron schedules (e.g., "every day at 2 AM UTC"), manual trigger (user clicks Run Now), file arrival triggers (run when a file lands in a cloud location), and continuous triggers (run continuously, waiting for new runs). You can also trigger a job via REST API.
Repair and Rerun
When a job run fails, the Repair and Rerun feature lets you retry the failed tasks without re running the entire job from the start. A repair preserves the original run ID and executes only the tasks that failed plus any downstream tasks that depend on them. This is much more efficient than restarting the whole pipeline and is the standard production practice for handling transient failures.
Task Values
Task values allow one task to pass data to downstream tasks in the same job run. Use dbutils.jobs.taskValues.set(key, value) in one task to set a value, and dbutils.jobs.taskValues.get(key, default) in another task to retrieve it. This enables dynamic pipelines where one task's output determines the behavior of downstream tasks.
Retries and Notifications
Each task can be configured with automatic retry logic: max retries, timeout behavior, and backoff strategy. Job level notifications send alerts on job start, success, or failure via email, webhook, or PagerDuty. This keeps operators informed and can trigger downstream systems when pipelines complete.
Job Clusters vs Existing Clusters
A job cluster is ephemeral: provisioned when the job run starts and terminated when it finishes. This is cost efficient and isolates job runs from each other. An existing cluster is a long lived cluster you manage separately. Use existing clusters for development and testing, but job clusters for production pipelines.
Concurrent Runs
You can set a max concurrent runs limit on a job to prevent too many instances from running simultaneously. This is useful if your downstream systems can't handle parallel executions or if you want to control resource consumption.
---
## Code Examples
Job definition with multi task DAG (YAML format for Databricks Asset Bundles):
```yaml
resources:
  jobs:
    data_pipeline_job:
      name: "Multi Task Data Pipeline"
      schedule:
        quartz_cron_expression: "0 0 2 * * ?"  # 2 AM daily
        timezone_id: "UTC"
      max_concurrent_runs: 1
      tasks:
        - task_key: "extract_data"
          notebook_task:
            notebook_path: "/Workspace/extract"
            base_parameters:
              source: "s3://my-bucket/raw"
          job_cluster_key: "job_cluster"
        - task_key: "transform_data"
          notebook_task:
            notebook_path: "/Workspace/transform"
          depends_on:
            - task_key: "extract_data"
          job_cluster_key: "job_cluster"
        - task_key: "quality_check"
          sql_task:
            file_path: "/Workspace/quality_check.sql"
          depends_on:
            - task_key: "transform_data"
          job_cluster_key: "job_cluster"
      job_clusters:
        - job_cluster_key: "job_cluster"
          new_cluster:
            spark_version: "15.3.x-scala2.12"
            node_type_id: "i3.xlarge"
            num_workers: 2
```
Using task values to share data between tasks (Python):
```python
# Task 1: Extract and set a task value
import datetime
from dbutils.jobs import TaskValues
# Perform extraction logic
records_processed = 150000
# Set the task value for downstream tasks
dbutils.jobs.taskValues.set("records_count", records_processed)
dbutils.jobs.taskValues.set("extraction_date", str(datetime.date.today()))
print(f"Extracted {records_processed} records")
```
In a downstream task, retrieve the values:
```python
# Task 2: Retrieve and use task values
records_count = dbutils.jobs.taskValues.get("records_count", "0")
extraction_date = dbutils.jobs.taskValues.get("extraction_date", "unknown")
print(f"Processing {records_count} records from {extraction_date}")
# Use these values to control downstream logic
if int(records_count) > 100000:
    print("Large dataset detected, using optimized transform")
else:
    print("Small dataset, using standard transform")
```
Repairing a failed run using the Databricks CLI:
```bash
# List recent runs for a job
databricks jobs list-runs --job-id 123
# View details of a failed run
databricks runs get --run-id 456
# Repair and rerun the failed run (preserves run ID)
databricks runs repair-run --run-id 456 --jar-params "param1" "param2"
# Check the status of the repair run
databricks runs get --run-id 456
```
Scheduling a job with cron expression (JSON format for REST API):
```json
{
  "name": "Daily ETL Job",
  "tasks": [
    {
      "task_key": "main_task",
      "notebook_task": {
        "notebook_path": "/Users/user@company.com/etl_notebook"
      },
      "job_cluster_key": "auto_cluster"
    }
  ],
  "schedule": {
    "quartz_cron_expression": "0 30 3 ? * MON-FRI",
    "timezone_id": "America/New_York"
  },
  "max_concurrent_runs": 1,
  "job_clusters": [
    {
      "job_cluster_key": "auto_cluster",
      "new_cluster": {
        "spark_version": "15.3.x-scala2.12",
        "node_type_id": "i3.xlarge",
        "num_workers": 2
      }
    }
  ]
}
```
---
## Common Exam Scenarios
Scenario 1: A multi task job fails at the quality check stage. The upstream extract and transform tasks completed successfully. You want to rerun the pipeline with minimal compute waste.
The correct approach is to use the Repair and Rerun feature on the failed run. This will execute only the quality check task (and any tasks downstream from it) without re executing the extract and transform tasks that already succeeded. This preserves the run ID and minimizes wasted cluster time. Simply clicking "Run Now" on the job would restart the entire pipeline from scratch, which is inefficient.
Scenario 2: Your extract task outputs the number of records processed. You want the transform task to behave differently depending on whether the extract loaded more than 1 million rows.
You use task values: the extract task calls dbutils.jobs.taskValues.set("record_count", 1500000) to store the count. The transform task retrieves it with dbutils.jobs.taskValues.get("record_count") and adjusts its parallelism or algorithm accordingly. This enables dynamic pipeline logic without relying on external state or databases.
Scenario 3: You need to choose between using a job cluster versus an existing cluster for your production pipeline. Your company wants to minimize costs but also ensure job isolation.
For production pipelines, job clusters are the best practice. Each job run gets its own ephemeral cluster that is provisioned at start and terminated at end. This isolates job runs from each other (no resource contention between concurrent jobs), eliminates the need to manage cluster uptime, and often reduces cost compared to keeping an always on cluster. Existing clusters are useful for development and ad hoc work where you want immediate cluster availability.
---
## Key Takeaways
- Lakeflow Jobs orchestrate data pipelines as DAGs of tasks with explicit dependencies. Multiple task types (Notebook, Python, SQL, Lakeflow Pipeline, dbt, etc.) can be mixed in a single job.
- Repair and rerun is the production standard for handling job failures. It reruns only failed tasks and their downstream dependents, preserving the original run ID and saving compute.
- Task values enable dynamic pipelines where one task can pass data to downstream tasks using dbutils.jobs.taskValues methods without external coordination.
- Job clusters are ephemeral and provisioned per run, making them cost effective and isolating production pipelines. Use existing clusters for development.
- Jobs can be triggered via cron schedule, manual trigger, file arrival, continuous mode, or REST API. Notifications and retries keep operators informed and handle transient failures.
---
## Gotchas and Tips
Repair and Rerun Preserves Run ID: When you repair a failed run, the original run ID is preserved and new run attempts are appended to the same run's history. This is different from clicking "Run Now" which creates a brand new run. If you need to track pipeline execution, understand that repairs are part of the same logical run.
Task Values Are Ephemeral: Task values exist only during a single job run and are not persisted to storage. They are ideal for passing small amounts of data between tasks in the same run, but do not use them to store permanent state. Each new job run has a clean slate.
Concurrent Runs Limit Can Queue Jobs: If you set max_concurrent_runs to 1 and a job run takes longer than your schedule interval, the next scheduled run will queue and wait. This can lead to pipeline backlog. Monitor job duration and adjust the schedule or concurrency limit accordingly.
---
## Links and Resources
- **[Lakeflow Jobs](https://docs.databricks.com/aws/en/jobs/)** — Main documentation for scheduling and orchestrating workflows.
- **[Configure and edit Lakeflow Jobs](https://docs.databricks.com/aws/en/jobs/configure-job)** — Setting up jobs with tasks, dependencies, and retry policies.
- **[Automating jobs with schedules and triggers](https://docs.databricks.com/aws/en/jobs/triggers)** — Time based and event based triggers for automated execution.