## Topic Overview
The **Medallion Architecture** (also called multi hop architecture) is a data design pattern that organizes your lakehouse into three distinct layers, each serving a specific purpose in the data pipeline. The Bronze layer captures raw, unprocessed data exactly as it arrives from source systems. The Silver layer contains cleaned, deduplicated, and conformed data that has been validated and standardized. The Gold layer holds business level aggregations, dimensional models, and analytics ready datasets prepared for BI tools and reporting.
This architecture is a foundational concept for the Databricks Certified Data Engineer Associate exam because it represents how real world data pipelines are built in enterprise lakehouse environments. Each layer enforces data quality at its stage, making it easy to identify where issues occur and to reprocess data if needed. The Medallion pattern works seamlessly with Databricks tools like Auto Loader for ingestion, Lakeflow Spark Declarative Pipelines for transformation, and Unity Catalog for governance.
The beauty of Medallion is simplicity combined with power. You don't need complex orchestration to implement it. Start with a raw data ingestion layer, add cleanup logic in the middle, and expose clean aggregates at the end. Each layer becomes a Delta table in your Unity Catalog, creating a clear lineage for governance and auditing. You can support both batch and streaming workloads within the same architecture, adapting your pipeline to how data actually arrives.
---
## Key Concepts
**Bronze Layer: Raw Ingestion**
The Bronze layer is where all raw data lands. It's append only, meaning data is never updated or deleted, only new records are added. This preserves the complete history of all data received from source systems. The Bronze layer uses tools like Auto Loader for streaming ingestion or COPY INTO for batch uploads. The goal is speed and completeness, not cleanliness. Data may have duplicates, null values, inconsistent formatting, and mixed data types. You preserve the data exactly as received because you might need to debug issues upstream. Bronze tables are typically partitioned by arrival date or source system for efficient querying and cleanup.
**Silver Layer: Cleaned and Conformed**
The Silver layer applies data quality rules and transformations to Bronze data. Here you deduplicate records, filter out invalid rows, standardize column names and data types, handle null values, and apply business logic rules. The Silver layer is where you join reference data to add context (for example, joining customer dimensions to a transactions table). Data in Silver is conformed meaning it follows organizational standards for naming, formatting, and structure. Silver tables are typically still quite granular (one row per business event) but clean and ready for analysis. This is also where you might quarantine bad data that fails quality checks, routing invalid records to a separate table for investigation.
**Gold Layer: Business Aggregates**
The Gold layer contains business level datasets. Data is heavily aggregated, modeled for specific use cases, and optimized for reporting and BI tools. Gold tables often follow a star schema or fact and dimension model. Examples include daily revenue summaries, customer lifetime value calculations, or a fact table of orders joined with dimension tables for products and dates. The Gold layer is what end users and BI tools query most frequently. Tables are typically heavily indexed and partitioned for fast query performance. You create Gold tables by aggregating and joining clean Silver data, computing business metrics and KPIs.
**Streaming Through the Layers**
Medallion Architecture supports real time streaming data. Bronze can ingest streaming events using Auto Loader (which internally uses Spark Structured Streaming). Data flows through Silver transformations in real time, and Gold aggregations update continuously. Lakeflow Spark Declarative Pipelines make this easy by letting you define a DAG of tables where each table can be either a streaming table or materialized view. A streaming table continuously processes new input data. A materialized view recomputes based on upstream changes. This allows you to push data through all three layers with low latency while maintaining the multi hop architecture benefits.
**Lakeflow Spark Declarative Pipelines and Medallion Architecture**
Lakeflow Spark Declarative Pipelines are built specifically to implement the Medallion pattern. You declare your pipeline as a set of SQL statements, each creating or updating a table. You mark Bronze tables as streaming tables ingesting from external sources. Silver tables transform Bronze data. Gold tables aggregate Silver data. Lakeflow automatically builds the dependency graph, runs only the necessary updates when source data changes, and provides built in error handling and recovery. The declarative approach means you focus on the data transformations, not on orchestration logic. Lakeflow also tracks data lineage through the layers automatically, which is crucial for governance and debugging.
---
## Code Examples
### Bronze Layer: Auto Loader Ingestion (Python)
```python
from pyspark.sql.types import StructType, StructField, StringType, IntegerType
# Define schema for incoming JSON events
schema = StructType([
    StructField("customer_id", StringType()),
    StructField("order_id", StringType()),
    StructField("amount", IntegerType()),
    StructField("timestamp", StringType())
])
# Auto Loader ingests raw JSON into bronze table
(spark.readStream
    .format("cloudFiles")
    .option("cloudFiles.format", "json")
    .option("cloudFiles.schemaLocation", "/tmp/schema_location")
    .schema(schema)
    .load("/mnt/raw/orders/")
    .writeStream
    .format("delta")
    .option("checkpointLocation", "/tmp/checkpoint")
    .option("mergeSchema", "true")
    .table("bronze_orders")
)
```
This code uses Auto Loader to stream JSON files from cloud storage into the bronze_orders table. Auto Loader automatically detects new files, parses them, and appends rows to the Delta table. The checkpointLocation tracks which files have been processed, preventing duplicates. mergeSchema allows the schema to evolve as new fields appear in incoming data.
### Silver Layer: Cleaning and Deduplication (SQL)
```sql
CREATE OR REPLACE TABLE silver_orders AS
WITH deduplicated AS (
  SELECT 
    customer_id,
    order_id,
    amount,
    CAST(timestamp AS TIMESTAMP) AS order_timestamp,
    ROW_NUMBER() OVER (PARTITION BY order_id ORDER BY timestamp DESC) AS rn
  FROM bronze_orders
  WHERE customer_id IS NOT NULL
    AND order_id IS NOT NULL
    AND amount > 0
)
SELECT 
  customer_id,
  order_id,
  amount,
  order_timestamp,
  CURRENT_TIMESTAMP() AS processed_at
FROM deduplicated
WHERE rn = 1;
```
This SQL transformation reads from bronze_orders and applies data quality rules. It filters out rows with null keys or invalid amounts, casts timestamp to the correct type, deduplicates using a window function (keeping the most recent record per order_id), and adds a processed_at column for audit trails. The result is a clean, conformed silver table.
### Gold Layer: Business Aggregation (SQL)
```sql
CREATE OR REPLACE TABLE gold_daily_revenue AS
SELECT 
  CAST(order_timestamp AS DATE) AS order_date,
  COUNT(DISTINCT customer_id) AS num_customers,
  COUNT(DISTINCT order_id) AS num_orders,
  SUM(amount) AS total_revenue,
  AVG(amount) AS avg_order_value,
  MIN(amount) AS min_order_value,
  MAX(amount) AS max_order_value
FROM silver_orders
GROUP BY CAST(order_timestamp AS DATE)
ORDER BY order_date DESC;
```
This Gold table aggregates clean Silver data to produce daily revenue metrics. A BI tool can query this table directly to show dashboards without needing to process millions of order rows. The aggregation reduces data volume and makes queries fast. You might create many Gold tables for different business perspectives (revenue, customer metrics, product analysis, etc.).
### Complete Medallion Pipeline in Lakeflow Spark Declarative Pipelines (SQL)
```sql
-- Bronze: streaming table ingesting raw data
CREATE STREAMING TABLE bronze_orders
COMMENT "Raw orders data from source system"
AS
SELECT *
FROM cloud_files(
  "/mnt/raw/orders",
  "json",
  map("cloudFiles.schemaLocation", "/tmp/schema")
);
-- Silver: materialized view with cleaning logic
CREATE MATERIALIZED VIEW silver_orders
COMMENT "Cleaned and deduplicated orders"
AS
WITH deduplicated AS (
  SELECT 
    customer_id,
    order_id,
    amount,
    CAST(timestamp AS TIMESTAMP) AS order_timestamp,
    ROW_NUMBER() OVER (PARTITION BY order_id ORDER BY timestamp DESC) AS rn
  FROM STREAM(bronze_orders)
  WHERE customer_id IS NOT NULL
    AND amount > 0
)
SELECT 
  customer_id,
  order_id,
  amount,
  order_timestamp,
  CURRENT_TIMESTAMP() AS processed_at
FROM deduplicated
WHERE rn = 1;
-- Gold: materialized view with business aggregations
CREATE MATERIALIZED VIEW gold_daily_revenue
COMMENT "Daily revenue summary for reporting"
AS
SELECT 
  CAST(order_timestamp AS DATE) AS order_date,
  COUNT(DISTINCT customer_id) AS num_customers,
  COUNT(DISTINCT order_id) AS num_orders,
  SUM(amount) AS total_revenue
FROM silver_orders
GROUP BY CAST(order_timestamp AS DATE);
```
This Lakeflow pipeline declares all three layers in one SQL notebook. The Bronze table streams raw data using Auto Loader (via the `cloud_files()` function). Silver reads from Bronze, cleans the data, and outputs a materialized view. Gold reads from Silver, aggregates, and outputs daily metrics. Lakeflow automatically manages dependencies and updates tables incrementally as new data arrives. The STREAM() function tells Lakeflow to process only new rows since the last update.
---
## Common Exam Scenarios
**Scenario 1: Choosing the Right Layer for Data Quality Checks**
Your company receives customer transaction data from a payment processor. The raw JSON sometimes contains duplicate events (the same transaction appears twice) and occasionally missing customer_id values. Where should you implement deduplication and filtering logic? The answer is Silver. Bronze should capture raw data exactly as received so you can debug upstream issues. The Silver layer is specifically designed for applying data quality rules. You deduplicate in Silver, filter out invalid records, and standardize formats. This keeps Bronze pure and makes troubleshooting easier if something goes wrong.
**Scenario 2: Determining Which Layer Feeds Your BI Dashboard**
Your analytics team needs to build a dashboard showing top 10 customers by revenue for the current month. Should they query Bronze, Silver, or Gold? The answer is Gold. Gold tables are pre aggregated and modeled specifically for reporting. A Gold table with customer lifetime value or monthly revenue per customer would make this dashboard instant to render. If they query Silver, they're scanning individual transaction rows and doing aggregations in Tableau or Power BI (slower and more expensive on compute). Bronze is for ingestion only. Always expose pre aggregated, business ready Gold tables to BI tools for best performance.
**Scenario 3: Implementing Streaming Data Through Medallion**
Your company wants real time revenue dashboards that update every minute instead of batch daily reports. How do you adapt the Medallion Architecture? Use Lakeflow Spark Declarative Pipelines with streaming tables and materialized views. Make Bronze a streaming table reading from Auto Loader (continuously processing new events). Make Silver a materialized view that applies transformations to each new batch of Bronze data. Make Gold a materialized view that aggregates Silver data. Lakeflow automatically orchestrates the flow: when new events arrive in Bronze, they flow through Silver transformations, which trigger Gold aggregations. You get real time freshness without manual orchestration. The multi layer structure ensures data quality at each step even with continuous updates.
---
## Key Takeaways
- Medallion Architecture organizes data into three layers: Bronze (raw append only), Silver (cleaned and conformed), and Gold (aggregated for business use). Each layer serves a distinct purpose in the pipeline.
- Bronze tables preserve raw data as is. Never transform Bronze data. Use Auto Loader or COPY INTO for ingestion and always append. Keep Bronze immutable so you can troubleshoot issues.
- Silver is where data quality rules live. Deduplicate, filter invalid rows, standardize column names and types, join reference data. Silver data is clean and conformed but still granular.
- Gold tables are aggregated and modeled for specific business purposes (star schema, fact tables, summary tables). BI tools and dashboards query Gold, never Silver or Bronze.
- Lakeflow Spark Declarative Pipelines are the native Databricks way to implement Medallion. They handle orchestration, incremental processing, error recovery, and lineage tracking automatically.
---
## Gotchas and Tips
**⚠️ Never Apply Quality Rules in Bronze** A common mistake is filtering or transforming data as it lands in Bronze. Resist this. Bronze is intentionally raw. If you filter out "bad" records in Bronze, you lose the ability to audit what was filtered and why. If upstream changes their data format and you need to replay history, you've lost the original data. Keep Bronze immutable. Quality rules belong in Silver.
**⚠️ Gold Tables Are for Reporting, Not for Operational Queries** Highly aggregated Gold tables are perfect for dashboards but not for drilling down into details. If your BI dashboard needs "show me the transactions for customer X", you query Silver or Bronze, not a pre aggregated Gold table. Design Gold tables with your reporting use cases in mind, not as a general purpose view of the data.
**⚠️ Deduplication Logic Depends on Your Business Key** When you deduplicate in Silver, use the right business key. If you deduplicate on order_id but a customer places two identical orders with the same id (yes, it happens), you've lost real data. Define your business key carefully. Use ROW_NUMBER() with a clear PARTITION BY clause and ORDER BY logic to keep the record you want (usually most recent or earliest depending on the use case).
---
## Links and Resources
- **[What is the medallion lakehouse architecture?](https://docs.databricks.com/aws/en/lakehouse/medallion)** — Official guide to the Bronze, Silver, Gold layered data architecture.
- **[What is a data lakehouse?](https://docs.databricks.com/aws/en/lakehouse/)** — Overview of the lakehouse paradigm combining data lakes and data warehouses.
- **[Tutorial: Build an ETL pipeline](https://docs.databricks.com/aws/en/getting-started/data-pipeline-get-started)** — Hands on tutorial building a medallion architecture pipeline with Lakeflow.