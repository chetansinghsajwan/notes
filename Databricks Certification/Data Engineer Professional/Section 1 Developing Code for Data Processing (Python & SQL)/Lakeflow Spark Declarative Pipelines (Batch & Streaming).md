## Topic Overview
Lakeflow Spark Declarative Pipelines are a framework for building reliable, maintainable data pipelines on Databricks. Formerly known as Delta Live Tables (DLT), this framework lets you define transformations declaratively using SQL or Python, and Databricks handles scheduling, error handling, and quality monitoring automatically.
The key difference between Lakeflow and traditional Spark code is the declarative approach. Instead of writing imperative code that says how to compute results (collect, compute, write), you declare what you want to compute using decorators like @dlt.table or @dlt.view. Databricks analyzes your declarations, understands data dependencies, and orchestrates execution. This makes it easy to add expectations (data quality checks), handle recovery automatically, and trigger pipeline reruns only on affected datasets.
The Professional exam expects you to understand the syntax, the difference between batch and streaming tables, when to use views vs tables, how to define expectations, and the difference between triggered and continuous pipeline modes.
---
## Key Concepts
Declarative vs Imperative Pipelines
Imperative pipelines are traditional Spark code where you explicitly define each step: read data, transform, write output, etc. Declarative pipelines describe what results you want without specifying every step. Lakeflow Spark Declarative Pipelines use a declarative model. You annotate Python or SQL functions with @dlt.table or @dlt.view, Databricks infers dependencies and automatically manages orchestration, retries, and incremental updates. The declarative approach is more maintainable because your code is focused on transformation logic, not plumbing.
Tables vs Views in Lakeflow Pipelines
Lakeflow tables are materialized results written to storage. They persist across pipeline runs and are useful when you want to reuse the computed data multiple times or when the computation is expensive. Lakeflow views are temporary, computed on the fly during the pipeline run. Views are useful for intermediate transformations that are only used once. Views do not persist and are recomputed if referenced by downstream tables. Choose tables when results are expensive to recompute or frequently accessed; use views for temporary intermediate results.
Batch vs Streaming Tables
Batch tables process all available data each run. They are ideal for daily or hourly job patterns where you process a full dataset from scratch (or incrementally via Lakeflow logic). Streaming tables process data continuously as new records arrive from a streaming source like Kafka or Auto Loader. Streaming tables maintain state across runs and can keep track of what was already processed. Use batch tables for scheduled jobs; use streaming tables when data arrives continuously and you want real time or near real time processing.
Expectations for Data Quality
Expectations let you define quality rules directly in your Lakeflow pipeline code. You use the @dlt.expect* decorator to assert conditions on your data. For example, @dlt.expect("valid_email", "email like '%@%.%'") checks that the email column matches a pattern. Lakeflow tracks which records fail expectations and quarantines them or logs violations. You can choose to fail the pipeline if violations exceed a threshold or just log them for monitoring. This declarative quality checking is much cleaner than writing separate validation code.
---
## Code Examples
### Batch Table with SQL
```sql
-- Define a batch table in Lakeflow using SQL
CREATE OR REPLACE TABLE dlt.customer_orders
COMMENT "Cleaned customer orders"
AS
SELECT 
    order_id,
    customer_id,
    order_date,
    amount,
    CASE 
        WHEN amount < 0 THEN NULL
        ELSE amount
    END AS valid_amount
FROM dlt.raw_orders
WHERE order_date >= CURRENT_DATE() - INTERVAL 30 DAYS
```
### Streaming Table with Python and Expectations
```python
import dlt
from pyspark.sql.functions import col
# Define a streaming table with expectations
@dlt.table(
    name="events_stream",
    comment="Real time streaming events",
    table_properties={"quality": "silver"}
)
@dlt.expect("valid_user_id", "user_id IS NOT NULL")
@dlt.expect("positive_timestamp", "event_timestamp > 0")
def events_stream():
    # Auto Loader reads new files as they arrive
    return dlt.read_stream("cloud_files") \
        .select("user_id", "event_timestamp", "event_type", "data")
# Define a materialized view that transforms the stream
@dlt.table(
    name="events_enriched",
    comment="Enriched events with user info"
)
def events_enriched():
    events = dlt.read("events_stream")
    users = dlt.read("dim_users")
    return events.join(users, on="user_id", how="left")
```
### View for Intermediate Transformation
```sql
-- Views are temporary and recomputed each run
CREATE OR REPLACE VIEW dlt.orders_by_month AS
SELECT 
    DATE_TRUNC('month', order_date) AS month,
    COUNT(*) AS order_count,
    SUM(amount) AS total_revenue
FROM dlt.customer_orders
GROUP BY DATE_TRUNC('month', order_date);
-- This table uses the view above
CREATE OR REPLACE TABLE dlt.monthly_summary AS
SELECT 
    month,
    order_count,
    total_revenue,
    ROUND(total_revenue / order_count, 2) AS avg_order_value
FROM dlt.orders_by_month
```
---
## Common Exam Scenarios
A team is building a daily batch ingestion pipeline. Raw data arrives each day in cloud storage. They need to clean it and make it available to analysts. Should they use streaming tables or batch tables? Batch tables are the correct choice. Since data arrives daily in batches (not continuously), a batch pipeline that runs on a schedule processes all available data. Streaming tables are better for continuous data streams like event logs from Kafka. The exam expects you to recognize that streaming is not always better; batch is more appropriate for periodic data arrivals.
A Lakeflow pipeline defines multiple intermediate transformations that feed into a single final table. Should the intermediate steps be tables or views? They should be views. Views are temporary and do not persist. Because they are only used as inputs to the final table, materializing them would waste storage without benefit. The final result should be a table so it is available to downstream consumers. This choice optimizes for performance and storage efficiency.
A data quality requirement states that all customer IDs must be non null and all order amounts must be positive. How should this be enforced in a Lakeflow pipeline? Use expectations with @dlt.expect decorators. Define @dlt.expect("non_null_customer_id", "customer_id IS NOT NULL") and @dlt.expect("positive_amount", "amount > 0"). Lakeflow tracks violations and can quarantine bad records. This is cleaner and more maintainable than writing separate validation code outside the pipeline.
---
## Key Takeaways
- Lakeflow Spark Declarative Pipelines use decorators (@dlt.table, @dlt.view) to define transformations declaratively.
- Use tables for results you want to persist and reuse; use views for temporary intermediate computations.
- Batch tables process scheduled batches of data; streaming tables process continuous data arrivals.
- Define quality rules with @dlt.expect to declaratively specify data quality constraints.
- Lakeflow handles dependency tracking automatically based on which tables/views you read from.
---
## Gotchas and Tips
⚠️ Lakeflow pipelines can run in triggered or continuous mode. Triggered pipelines run on demand or on a schedule. Continuous pipelines continuously ingest from streaming sources. Choose the mode based on your data arrival pattern.
⚠️ When a Lakeflow table definition changes (e.g., new column), downstream tables that depend on it are automatically invalidated and will be recomputed on the next pipeline run.
⚠️ Records that fail expectations can be quarantined instead of failing the entire pipeline. Configure this in the pipeline settings based on your quality tolerance.
---
## Links and Resources
- **[Lakeflow Spark Declarative Pipelines](https://docs.databricks.com/aws/en/ldp/)** — Building reliable batch and streaming ETL pipelines with declarative syntax.
- **[Lakeflow SDP Concepts](https://docs.databricks.com/aws/en/ldp/concepts)** — Core concepts including datasets, expectations, and pipeline modes.
- **[Streaming Tables](https://docs.databricks.com/aws/en/ldp/streaming-tables)** — Defining append-only streaming tables for incremental data processing.