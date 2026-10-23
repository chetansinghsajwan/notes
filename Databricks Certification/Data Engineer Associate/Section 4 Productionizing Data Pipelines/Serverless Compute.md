## Topic Overview
Serverless compute is Databricks managed infrastructure where you don't configure or manage clusters yourself. Instead, Databricks provisions and manages the compute on your behalf. This is a significant shift from traditional cluster based workflows where you manually create, configure, and tear down compute resources.
Serverless compute is available for several workloads: SQL Warehouses (the most common option for SQL queries and BI tools), Notebooks (for exploratory work and development), Jobs and Workflows (for scheduled ETL and automation), and Lakeflow Pipelines (for data pipeline orchestration).
The main advantages of serverless compute are instant startup (no waiting for cluster provisioning), automatic scaling handled by Databricks, no cluster management overhead, and pay per use billing with no idle costs. You simply define your workload and submit it. Databricks takes care of the rest.
---
## Key Concepts
What is Serverless
Serverless means Databricks provisions, configures, and scales compute resources automatically without you needing to manage the infrastructure. You focus on your code and data, Databricks handles cluster lifecycle. It's called serverless even though servers are involved behind the scenes because you never interact with server configuration directly.
Serverless SQL Warehouses
The most common serverless option on Databricks. Used for SQL queries, dashboards, BI tool connections, and analytics workloads. Serverless SQL Warehouses run Photon by default, which provides GPU acceleration for SQL operations. No cluster configuration needed. You simply execute SQL and the warehouse scales up or down automatically based on query load. Billing is based on compute usage.
Serverless for Jobs and Notebooks
You can run notebook tasks and Python tasks within jobs using serverless compute. Benefits include faster job startup (no cluster provisioning delay), automatic scaling based on task requirements, and simpler job configuration since you don't specify instance types or cluster sizes. This is ideal for production workflows where you want hands off execution and predictable startup times.
Serverless vs Classic Compute
Classic (self managed) compute gives you full control over instance types, cluster size, auto scaling policies, and runtime configuration. You manage cluster lifecycle, which adds operational overhead but provides fine grained control. Serverless abstracts these details. Serverless has faster startup times but may have higher per unit cost since you're paying for Databricks managed infrastructure. Classic gives you cost predictability if you have steady state workloads but requires hands on management.
Cost Considerations
Serverless billing is based on DBU consumption, measured while your workload runs. You don't pay for idle time since resources are released when not in use. This is advantageous for variable workloads with unpredictable demand. There are no costs for provisioning or cluster management. However, per DBU costs may be higher than classic compute. For sustained, steady state workloads, classic compute might be cheaper. Always compare your expected usage patterns.
Unity Catalog Requirement
Serverless compute requires Unity Catalog for data governance and access control. You cannot use the legacy Hive Metastore with serverless compute. All data accessed by serverless workloads must be registered in Unity Catalog. This is actually a benefit because Unity Catalog provides fine grained access control, audit logs, and data lineage tracking.
Region Availability
Serverless compute is not available in all regions. Check Databricks documentation for your region's availability. Common regions like US East, US West, and EU have serverless support, but some regional availability gaps exist. If serverless is not available in your required region, you must fall back to classic compute.
---
## Code Examples
### Example 1: Querying via Serverless SQL Warehouse
```sql
-- Execute SQL against a serverless SQL warehouse
-- No cluster provisioning needed, Databricks handles scaling
SELECT 
  customer_id,
  COUNT(*) as order_count,
  SUM(total_amount) as total_spent
FROM main.ecommerce.orders
WHERE order_date >= CURRENT_DATE - INTERVAL 30 DAY
GROUP BY customer_id
ORDER BY total_spent DESC
LIMIT 100;
-- Serverless SQL warehouse automatically:
-- - Provisions compute when query arrives
-- - Scales up if needed for large result sets
-- - Scales down when query completes
-- - Applies Photon acceleration by default
```
### Example 2: Job Configuration Using Serverless Compute
```yaml
# Databricks Asset Bundle job configuration using serverless compute
resources:
  jobs:
    daily_etl_pipeline:
      name: Daily ETL Pipeline
      tasks:
        - task_key: ingest_data
          notebook_task:
            notebook_path: /pipelines/ingest_raw_data
          # No cluster configuration needed for serverless
          # Databricks automatically handles compute provisioning
          max_concurrent_runs: 1
          
        - task_key: transform_data
          depends_on:
            - task_key: ingest_data
          notebook_task:
            notebook_path: /pipelines/transform_bronze_to_silver
          # Serverless automatically scales for this task
          
      schedule:
        quartz_cron_expression: "0 0 * * *"
        timezone_id: America/New_York
# With serverless:
# - Job starts immediately (no cluster wait time)
# - Databricks provisions smallest compute needed
# - Scales if tasks require more resources
# - Releases resources when done
# - No idle compute costs
```
### Example 3: Classic Cluster Configuration (For Comparison)
```yaml
# Classic cluster configuration (more configuration overhead)
resources:
  jobs:
    daily_etl_pipeline:
      name: Daily ETL Pipeline
      clusters:
        - cluster_key: main_cluster
          num_workers: 2
          spark_version: "14.3.x-scala2.12"
          node_type_id: "i3.xlarge"
          aws_attributes:
            availability: SPOT
          spark_conf:
            spark.databricks.optimizer.dynamicFilePruning: "true"
      tasks:
        - task_key: ingest_data
          notebook_task:
            notebook_path: /pipelines/ingest_raw_data
          cluster_key: main_cluster
        - task_key: transform_data
          depends_on:
            - task_key: ingest_data
          notebook_task:
            notebook_path: /pipelines/transform_bronze_to_silver
          cluster_key: main_cluster
# With classic cluster:
# - You specify instance types and worker count
# - Longer startup (cluster provision + initialization)
# - Fixed cost structure based on configured cluster
# - You manage scaling policies
# - Idle costs if cluster sits unused
# - More control, more complexity
```
---
## Common Exam Scenarios
### Scenario 1: Choosing Compute Type for a Dashboard
An organization needs to publish an interactive dashboard that runs ad hoc SQL queries from business users throughout the day. Query patterns are unpredictable and the dashboard may sit unused for hours. Which compute should they use?
Answer: Serverless SQL Warehouse. The unpredictable usage pattern means you don't want to pay for idle time. Serverless automatically scales based on query demand and releases resources when not in use, eliminating idle costs. Classic compute would be wasteful here since the cluster would sit provisioned even when no queries run.
### Scenario 2: Minimizing Job Startup Time
A data team has a critical job that must complete within a tight SLA. Currently using classic compute, job startup takes 5 minutes just for cluster provisioning. The actual job runs for 3 minutes. They need to minimize total runtime. What should they change?
Answer: Switch to serverless compute for the job tasks. Serverless has near instant startup (seconds instead of minutes) since Databricks provision compute on demand. This reduces total job time from 8 minutes to approximately 3 plus few seconds, meeting the SLA much more reliably.
### Scenario 3: Governance Requirement Impact on Compute Choice
A company wants to use serverless compute for their new ETL pipelines but still uses Hive Metastore for data governance. Can they use serverless?
Answer: No. Serverless compute requires Unity Catalog for governance. They cannot use Hive Metastore with serverless. They must either migrate to Unity Catalog (which is the recommended path) or stay with classic compute if they're not ready to migrate yet.
---
## Key Takeaways
- Serverless compute is Databricks managed infrastructure with no cluster configuration needed. Available for SQL Warehouses, Jobs, Notebooks, and Lakeflow Pipelines.
- Instant startup and automatic scaling are major benefits. No idle costs since resources are released when not in use. Billing is based on DBU consumption only during active execution.
- Serverless SQL Warehouses run Photon by default, providing automatic SQL acceleration without any additional configuration.
- Unity Catalog is mandatory for serverless compute. Hive Metastore is not compatible, so you must migrate or use classic compute.
- Choose serverless for variable or unpredictable workloads, choose classic if you need fine grained control over instance types and configuration.
---
## Gotchas and Tips
Unity Catalog is not optional. If your workspace still uses Hive Metastore, you cannot use serverless compute at all. This is a hard requirement, not a nice to have. You must migrate to UC first.
Not all regions support serverless. Before architecting a solution with serverless, verify your target region supports it. Exam questions may test this by asking what to do if serverless is unavailable in a specific region.
Per DBU cost for serverless is higher than classic compute. For high volume, always on workloads, classic compute may be more cost effective. Don't assume serverless is always cheaper. Calculate based on your actual usage pattern.
---
## Links and Resources
- **[Run jobs with serverless compute for workflows](https://docs.databricks.com/aws/en/jobs/run-serverless-jobs)** — Running jobs without managing infrastructure using serverless compute.
- **[Serverless compute for notebooks](https://docs.databricks.com/aws/en/compute/serverless/notebooks)** — Using serverless compute for interactive development.
- **[Connect to serverless compute](https://docs.databricks.com/aws/en/admin/workspace-settings/serverless)** — Admin settings for enabling serverless in your workspace.