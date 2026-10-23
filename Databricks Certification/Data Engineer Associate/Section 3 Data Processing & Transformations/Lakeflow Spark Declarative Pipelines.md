## Topic Overview
Lakeflow Spark Declarative Pipelines (formerly known as Delta Live Tables) is a declarative framework for building reliable data pipelines on Databricks. Instead of writing imperative Spark code where you specify every step, you declare your desired end state—your target tables and their definitions—and the system handles the rest. This includes orchestration, dependency management, error handling, and data quality validation.
The declarative approach means you focus on what data you want, not how to compute it. Lakeflow Spark Declarative Pipelines automatically figures out the optimal execution order, handles retries, and ensures your data quality rules are enforced. This is especially powerful for building medallion architecture pipelines (Bronze, Silver, Gold) where you have many interdependent transformations.
Pipelines are defined in notebooks using SQL or Python and then deployed as pipeline objects in Databricks. You run them in either Development mode (for iteration) or Production mode (for reliable scheduled execution). The exam expects you to understand how to design pipelines, choose between Streaming Tables and Materialized Views, apply data quality expectations, and troubleshoot pipeline failures.
---
## Key Concepts
What are Lakeflow Spark Declarative Pipelines?
Lakeflow Spark Declarative Pipelines is a declarative data pipeline framework where you define tables using SQL `CREATE STREAMING TABLE` and `CREATE MATERIALIZED VIEW` statements (or Python equivalents). The framework automatically manages dependencies, scheduling, error handling, and data quality. Key benefits include simplified code, automatic optimization, built in data quality constraints, and production ready reliability out of the box.
Streaming Tables vs Materialized Views
Streaming Tables are append only tables optimized for incremental and streaming data. They process only new data since the last run (checkpoint), making them efficient for high volume sources like message buses. Materialized Views are recomputed from scratch on each pipeline run and are ideal for aggregations, deduplication, and transformations. Choose Streaming Tables for your Bronze layer (raw data ingestion) and consider Materialized Views for Silver and Gold layers depending on whether you need incremental processing or full recomputation.
Expectations (data quality constraints)
Expectations are data quality rules defined inline in your table definition using the syntax `CONSTRAINT name EXPECT (condition)`. Each expectation has an action: WARN logs violations to the event log but lets rows through, DROP filters out rows that violate the constraint, and FAIL stops the entire pipeline if violations are detected. Expectations are checked as data is processed, catching quality issues early in the pipeline.
Triggered vs Continuous mode
Triggered pipelines run once and complete, then stop. They are ideal for scheduled batch jobs (e.g., daily or hourly runs). Continuous pipelines run indefinitely, processing data as it arrives, making them suitable for real time streaming scenarios. Continuous pipelines have lower latency but consume compute resources continuously. Choose based on your use case: batch transformations use Triggered mode, streaming data ingestion uses Continuous mode.
Development vs Production mode
Development mode allows faster iteration: it enables retries on transient failures, uses smaller clusters, and is suited for testing and development. Production mode is optimized for reliability: it uses larger clusters if needed, has stricter error handling, and is configured for scheduled execution. You typically develop your pipeline in a Development cluster and then deploy it to run in Production mode on a schedule.
Pipeline dependencies and DAG resolution
Lakeflow Spark Declarative Pipelines automatically determines dependencies by analyzing your table definitions. If Table B reads from Table A, the pipeline knows A must compute first. The system builds a directed acyclic graph (DAG) and executes tables in the correct order. You don't manually specify orchestration—this automatic dependency resolution is a key advantage over traditional Spark jobs.
---
## Code Examples
**Creating a Streaming Table with Auto Loader**
```sql
CREATE STREAMING TABLE bronze_orders
COMMENT "Raw orders from cloud storage"
AS
SELECT *
FROM cloud_files(
  "s3://my-bucket/orders/",
  "csv",
  map(
    "cloudFiles.format", "csv",
    "header", "true"
  )
)
```
**Creating a Materialized View with Aggregation**
```sql
CREATE MATERIALIZED VIEW silver_orders_daily
COMMENT "Daily order summary"
AS
SELECT
  DATE(order_date) AS order_day,
  COUNT(*) AS total_orders,
  SUM(order_amount) AS total_revenue,
  AVG(order_amount) AS avg_order_amount
FROM bronze_orders
WHERE order_date IS NOT NULL
GROUP BY DATE(order_date)
```
**Expectations / Data Quality Constraints**
```sql
CREATE STREAMING TABLE silver_customers
CONSTRAINT valid_email EXPECT (email LIKE '%@%.%') ON VIOLATION DROP
CONSTRAINT positive_age EXPECT (age > 0) ON VIOLATION FAIL
CONSTRAINT not_null_id EXPECT (customer_id IS NOT NULL) ON VIOLATION WARN
AS
SELECT *
FROM bronze_customers
```
**Python API Equivalent for Creating Streaming Tables**
```python
from databricks.sdk.python.dlt import create_streaming_table, expect
from pyspark.sql.functions import col
@create_streaming_table
def bronze_orders():
    return (
        spark.readStream.format("cloudFiles")
        .option("cloudFiles.format", "csv")
        .option("header", "true")
        .load("s3://my-bucket/orders/")
    )
@create_streaming_table
@expect("valid_amount", "order_amount > 0", "DROP")
def silver_orders():
    return spark.sql("SELECT * FROM bronze_orders")
```
**Full Medallion Architecture Example (Bronze / Silver / Gold)**
```sql
-- BRONZE LAYER: Raw data ingestion
CREATE STREAMING TABLE bronze_events
AS
SELECT *
FROM cloud_files(
  "s3://events/raw/",
  "json"
);
-- SILVER LAYER: Cleaned and deduplicated
CREATE STREAMING TABLE silver_events
CONSTRAINT valid_timestamp EXPECT (event_time IS NOT NULL) ON VIOLATION DROP
AS
SELECT
  event_id,
  event_time,
  user_id,
  event_type,
  _metadata.file_modification_time AS ingestion_time
FROM bronze_events
QUALIFY ROW_NUMBER() OVER (PARTITION BY event_id ORDER BY _metadata.file_modification_time DESC) = 1;
-- GOLD LAYER: Business metrics
CREATE MATERIALIZED VIEW gold_user_activity
AS
SELECT
  user_id,
  DATE(event_time) AS activity_date,
  COUNT(*) AS event_count,
  COUNT(DISTINCT event_type) AS distinct_events
FROM silver_events
GROUP BY user_id, DATE(event_time);
```
---
## Common Exam Scenarios
**Scenario 1: Choosing Between Streaming Table and Materialized View for Silver Layer**
You have a Bronze layer with raw customer data arriving every minute from an S3 bucket. Your Silver layer needs to deduplicate records by customer_id and keep only the latest entry. Your Gold layer reports use this deduplicated data for daily aggregations. Should you use a Streaming Table or Materialized View for the Silver layer deduplication?
Answer: Use a Materialized View for the Silver deduplication layer. Although the Bronze layer is a Streaming Table (appending raw data), the Silver layer performs deduplication using ROW_NUMBER(), which requires full dataset access. Deduplication is a transformation best handled by a Materialized View that recomputes the full state. The Streaming Table would be inefficient here because deduplication cannot be done incrementally you need to see all records to identify the latest one per customer. The Materialized View will recompute from the Bronze Streaming Table on each pipeline run, ensuring accurate deduplication.
**Scenario 2: Handling Data Quality Violations in a Pipeline**
Your pipeline ingests transaction data with an expectation that transaction_amount > 0. Recently, a data source bug sent transactions with null amounts. You want to log these violations for investigation but prevent bad data from reaching downstream tables. Which constraint action would you use: WARN, DROP, or FAIL?
Answer: Use DROP. WARN only logs violations and still passes bad data downstream. FAIL would stop the entire pipeline, blocking valid transactions too. DROP filters out rows that fail the constraint, so valid transactions continue flowing while bad ones are removed. The violations are still logged in the pipeline event log, giving you a record of what was filtered. This is the right choice when you want to enforce a quality rule without halting the pipeline.
**Scenario 3: Pipeline Execution Order with Dependencies**
You define three tables in a notebook: bronze_customers (reads from S3), silver_customers (reads from bronze_customers), and gold_customer_metrics (reads from silver_customers). You don't explicitly order the table definitions in your notebook. Does Lakeflow Spark Declarative Pipelines still execute them in the correct order?
Answer: Yes. Lakeflow Spark Declarative Pipelines automatically analyzes table definitions to determine dependencies and builds a DAG. The order of table definitions in the notebook does not matter. The system will always execute bronze_customers first, then silver_customers, then gold_customer_metrics, regardless of how they appear in the notebook. This automatic dependency resolution is a core advantage you declare what you want, and the system figures out the execution order.
---
## Key Takeaways
- Lakeflow Spark Declarative Pipelines uses declarative SQL/Python to define tables, not imperative code. You declare the desired end state and the system handles orchestration.
- Use Streaming Tables for append only, incremental data (Bronze layer). Use Materialized Views for aggregations and full state computation (Silver and Gold layers).
- Expectations enforce data quality with three actions: WARN (log only), DROP (filter bad rows), FAIL (stop pipeline).
- Dependencies are automatic the system analyzes table definitions and executes in the correct order. You don't manually define DAGs.
- Triggered mode for batch jobs (runs once), Continuous mode for streaming (runs indefinitely). Development mode for iteration, Production mode for scheduled reliable execution.
---
## Gotchas and Tips
Streaming Tables are append only. If you need to update or delete records, use a Materialized View instead. Streaming Tables cannot handle upsert patterns use change data feed (CDF) or manual delta merge operations for that.
Don't overuse FAIL constraints. A single FAIL constraint will stop the entire pipeline if triggered. Use DROP or WARN for non critical quality issues, and reserve FAIL for truly critical data requirements that should block the pipeline.
Materialized Views recompute fully on each run. If you have a large Materialized View that does expensive aggregations, consider moving it to Continuous mode with Auto Scaling, or use a Streaming Table if possible. Full recomputation can be costly at scale.
---
## Links and Resources
- **[Lakeflow Spark Declarative Pipelines](https://docs.databricks.com/aws/en/ldp/)** — Main documentation page for Lakeflow Spark Declarative Pipelines (formerly DLT).
- **[Lakeflow Spark Declarative Pipelines concepts](https://docs.databricks.com/aws/en/ldp/concepts)** — Core concepts including datasets, flows, and pipeline modes.
- **[Develop pipeline code with Python](https://docs.databricks.com/aws/en/ldp/developer/python-dev)** — Python API reference for building declarative pipelines.
- **[Develop pipeline code with SQL](https://docs.databricks.com/aws/en/ldp/developer/sql-dev)** — SQL syntax for defining streaming tables and materialized views.