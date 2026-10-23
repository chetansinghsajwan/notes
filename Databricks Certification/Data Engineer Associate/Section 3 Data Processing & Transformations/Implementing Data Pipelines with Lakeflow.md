## Topic Overview
Lakeflow Spark Declarative Pipelines provide a declarative way to build data pipelines in Databricks. Instead of writing imperative code that describes how to transform data step by step, you define what data you want to process and what transformations to apply. The pipeline engine handles scheduling, fault tolerance, and incremental processing automatically. This makes it easier to build reliable, maintainable data pipelines that scale.
A Lakeflow pipeline consists of one or more notebooks that contain SQL or Python code defining tables. Each notebook contains table definitions using statements like `CREATE OR REFRESH STREAMING TABLE` or `CREATE OR REPLACE MATERIALIZED VIEW`. The pipeline orchestrates these notebooks, manages dependencies between tables, and handles data quality checks. Pipelines integrate directly with Unity Catalog, storing results in a specified catalog and schema with full governance and lineage tracking.
For the exam, you need to understand how to configure pipelines, implement change data capture with APPLY CHANGES, choose between SCD Type 1 and Type 2, understand refresh modes, monitor pipelines through event logs, and handle errors gracefully. You should also be able to work with multi notebook pipelines and understand how they integrate with Unity Catalog.
---
## Key Concepts
Pipeline Configuration and Setup
A Lakeflow pipeline is configured with a name, target catalog and schema (catalog.schema), cluster configuration, and paths to source notebooks. You create a pipeline through the Databricks UI, REST API, or Databricks Asset Bundles. The pipeline specifies which notebooks contain table definitions. When you trigger a pipeline run, Databricks executes the notebooks in dependency order, creates or updates the target tables, and tracks the run in the event log. You can configure the pipeline to run on a schedule, trigger on external events, or run manually.
APPLY CHANGES for Change Data Capture (CDC)
APPLY CHANGES is a Lakeflow operation that processes change data capture (CDC) feeds. It takes a stream of inserts, updates, and deletes from a source table (or view) and applies them to a target table. The syntax is `APPLY CHANGES INTO target_table FROM source_table KEYS (primary_key_cols) SEQUENCE BY (sequence_col)`. The `KEYS` clause specifies the column(s) that uniquely identify a record. The `SEQUENCE BY` clause specifies a column that orders the changes (usually a timestamp or version number). APPLY CHANGES handles idempotency, so replaying the same changes twice produces the same result.
SCD Type 1 vs Type 2
Slowly Changing Dimension (SCD) Type 1 overwrites old values with new ones. When a record is updated, the previous value is lost. This is the default behavior. SCD Type 2 tracks history by creating new rows for each change, keeping the old and new values. You activate SCD Type 2 by adding `STORED AS SCD TYPE 2` to the table definition. With Type 2, the target table gets additional columns like `__start_version`, `__end_version`, `__current` that track when a record was active. Choose Type 1 when you only care about the current state. Choose Type 2 when you need to analyze how data has changed over time.
Full Refresh vs Incremental Refresh
A full refresh reprocesses all data from the source. Every time the pipeline runs, it reads the entire source and rebuilds the target table. This is simple but expensive for large datasets. An incremental refresh processes only new data since the last successful run. Lakeflow tracks a checkpoint that marks how far the pipeline has progressed, so the next run only reads new records. Streaming tables in Lakeflow always use incremental refresh. Materialized views can be configured for full or incremental refresh depending on the query.
Pipeline Event Logs and Monitoring
Every pipeline run generates events that are stored in a system managed event log table. This table contains information about pipeline start and stop times, table updates, data quality check results, and any errors that occurred. The event log is located at `system.pipelines.lineage` or in a pipeline specific catalog/schema. You can query the event log to understand pipeline performance, debugging issues, and tracking data lineage. The event log includes timestamps, table names, row counts, and error details if a pipeline fails.
Multi Notebook Pipelines
A single pipeline can contain definitions from multiple notebooks. Each notebook defines one or more tables. The pipeline engine automatically resolves dependencies between tables across notebooks. If Notebook A defines a staging table and Notebook B defines a gold table that reads from that staging table, the pipeline will execute Notebook A first, then Notebook B. You organize notebooks logically by layer (bronze, silver, gold) or by business domain. Multi notebook pipelines make code more modular and easier to maintain than putting everything in a single notebook.
---
## Code Examples
**APPLY CHANGES with SCD Type 1 (Overwrite)**
```sql
-- In a Lakeflow pipeline notebook
CREATE OR REFRESH STREAMING TABLE customers AS
APPLY CHANGES INTO live.customers
FROM raw_customers_cdc
KEYS (customer_id)
SEQUENCE BY event_timestamp
WHEN MATCHED AND operation = 'DELETE' THEN DELETE
WHEN MATCHED THEN UPDATE SET *
WHEN NOT MATCHED THEN INSERT *
```
**APPLY CHANGES with SCD Type 2 (Track History)**
```sql
-- In a Lakeflow pipeline notebook
CREATE OR REFRESH STREAMING TABLE customers_history
STORED AS SCD TYPE 2 AS
APPLY CHANGES INTO live.customers_history
FROM raw_customers_cdc
KEYS (customer_id)
SEQUENCE BY event_timestamp
WHEN MATCHED AND operation = 'DELETE' THEN DELETE
WHEN MATCHED THEN UPDATE SET *
WHEN NOT MATCHED THEN INSERT *
```
**Multi Notebook Pipeline: Bronze Layer**
```sql
-- Notebook: 01_bronze_ingest.sql
-- Ingests raw data from cloud storage
CREATE OR REFRESH STREAMING TABLE orders_bronze AS
SELECT
  *,
  _metadata.file_path,
  _metadata.file_modification_time
FROM cloud_files(
  '/mnt/raw/orders',
  'parquet',
  map('cloudFiles.schemaLocation', '/mnt/checkpoints/orders')
)
```
**Multi Notebook Pipeline: Silver Layer (depends on Bronze)**
```sql
-- Notebook: 02_silver_transform.sql
-- Transforms and cleanses bronze data
CREATE OR REFRESH STREAMING TABLE orders_silver AS
SELECT
  order_id,
  customer_id,
  order_date,
  amount,
  CAST(amount AS DECIMAL(10, 2)) AS amount_clean,
  _metadata.file_path,
  CURRENT_TIMESTAMP() AS processed_at
FROM live.orders_bronze
WHERE amount > 0
AND order_date IS NOT NULL
```
**Querying Pipeline Event Logs**
```sql
-- Query pipeline execution history
SELECT
  event_id,
  timestamp,
  details:table_name AS table_name,
  details:rows_written AS rows_written,
  details:state AS state,
  details:error_msg AS error_msg
FROM system.pipelines.lineage
WHERE details:pipeline_id = 'your_pipeline_id'
ORDER BY timestamp DESC
LIMIT 10
```
**Python API: APPLY CHANGES (PySpark)**
```python
# In a Lakeflow pipeline notebook using PySpark
from pyspark.sql import functions as F
# Read CDC feed (example: from Delta table)
cdc_df = spark.readStream.option(
    "ignoreChanges", "true"
).table("raw.customers_cdc")
# Write to target with APPLY CHANGES
(cdc_df
  .writeStream
  .option("mergeSchema", "true")
  .mode("append")
  .option("checkpointLocation", "/checkpoints/customers")
  .table("silver.customers")
)
```
---
## Common Exam Scenarios
**Scenario 1: Choosing Between SCD Type 1 and Type 2**
Your organization tracks customer data. The source system sends updates whenever a customer's address or phone number changes. You are building a Lakeflow pipeline to load this data into a silver layer table. You need to support two use cases: (1) a dashboard that shows the current state of all customers, and (2) an audit report showing when each customer's address changed. Which SCD type should you use?
The correct answer is SCD Type 2. Type 2 preserves history, so you can track when address changes occurred and generate audit reports. For the dashboard, you can filter to `__current = true` to get the current state. If you used Type 1, the old address would be overwritten and the audit trail would be lost. Type 1 is fine if you only need the current state.
**Scenario 2: Handling Data Dependency Failures in Multi Notebook Pipelines**
Your pipeline consists of three notebooks: (1) Bronze layer that reads from cloud storage, (2) Silver layer that cleans the bronze data, (3) Gold layer that creates aggregations for reporting. The Silver notebook fails due to a schema mismatch in the source data. What happens to the Gold layer?
The correct answer is the Gold layer does not run. Lakeflow detects the dependency: Gold depends on Silver, which depends on Bronze. When Silver fails, the pipeline stops and does not attempt to run Gold. This prevents cascading errors and corrupt data in downstream tables. You must fix the schema mismatch in the Silver notebook and retry the pipeline.
**Scenario 3: Incremental Processing with APPLY CHANGES**
Your source system sends CDC records to a Delta table at `raw.customer_changes`. Each row has an `operation` column (INSERT, UPDATE, DELETE) and an `event_time` timestamp. You run the Lakeflow pipeline every hour. On the first run, it processes 1,000 changes. On the second run an hour later, there are 800 new changes. How many total rows does the pipeline read from `raw.customer_changes` on the second run?
The correct answer is approximately 800 rows (only the new changes). Lakeflow maintains a checkpoint that tracks the `event_time` of the last processed record. On the second run, it reads only records with `event_time` later than the checkpoint. This incremental approach scales to large datasets. If you needed to reprocess all changes, you could reset the checkpoint, but that is rare.
---
## Key Takeaways
Lakeflow Spark Declarative Pipelines simplify data pipeline creation by letting you declaratively define tables instead of writing orchestration code. The pipeline engine handles dependencies, scheduling, and fault tolerance.
APPLY CHANGES processes CDC feeds from a source table and applies inserts, updates, and deletes to a target table. The KEYS clause specifies the primary key and SEQUENCE BY specifies the ordering column.
Use SCD Type 1 when you only need the current state of data. Use SCD Type 2 when you need to track history and analyze how data has changed over time.
Incremental refresh processes only new data since the last checkpoint, making pipelines efficient for large datasets. Full refresh reprocesses all data and is useful for rebuilds or corrections.
Pipeline event logs provide visibility into runs, row counts, data quality results, and errors. Use them to monitor pipeline health and debug issues.
---
## Gotchas and Tips
**Warning: APPLY CHANGES Requires Ordering**
If your source data does not have a reliable sequence column (timestamp or version), APPLY CHANGES may apply changes in the wrong order, leading to data corruption. Always ensure the SEQUENCE BY column accurately reflects the order in which changes occurred.
**Warning: SCD Type 2 Storage Overhead**
SCD Type 2 tables use additional storage because they keep all historical versions. For tables with frequent updates and long retention, this can become expensive. Monitor table size and consider archiving old versions.
**Warning: Checkpoint Reset Destroys State**
If you manually reset a pipeline checkpoint, the next run will reprocess all data from the beginning. This is rarely needed and can cause duplicates. Only reset checkpoints if you are intentionally rebuilding the table.
---
## Links and Resources
- **[Develop Lakeflow Spark Declarative Pipelines](https://docs.databricks.com/aws/en/ldp/develop)** — Guide to developing and testing pipeline code.
- **[Tutorial: Build an ETL pipeline with Lakeflow](https://docs.databricks.com/aws/en/getting-started/data-pipeline-get-started)** — Hands on walkthrough building a complete data pipeline.
- **[Run pipelines in a workflow](https://docs.databricks.com/aws/en/ldp/workflows)** — How to orchestrate Lakeflow pipelines using Lakeflow Jobs.