## Topic Overview
Auto Loader syntax is the practical layer of Auto Loader functionality. You need to know the exact options, configuration parameters, and the difference between using Auto Loader in Python streaming code versus SQL based Lakeflow Spark Declarative Pipelines. The syntax also changes depending on whether you're doing a one time batch load with COPY INTO or continuous streaming ingestion.
The core entry point for Auto Loader in PySpark is spark.readStream.format("cloudFiles"). From there you chain options for format, schema location, schema hints, and how you want to handle schema evolution. Each option controls a specific behavior, and missing options like schema location can cause your pipeline to fail.
Understanding when to use COPY INTO versus Auto Loader is also critical. COPY INTO works for one time or batch loads, while Auto Loader is built for continuous streaming ingestion. They have different syntax and serve different patterns, but candidates often mix them up.
---
## Key Concepts
cloudFiles Format and Basic Syntax
Auto Loader is accessed through the cloudFiles format. You call spark.readStream.format("cloudFiles") and then chain options. The most important option is .option("cloudFiles.format", "json") (or the file format you're reading: parquet, csv, avro, orc, text, binary). This tells Auto Loader what file type to expect.
Schema Location and Why It Matters
Schema location is required for streaming Auto Loader pipelines. You provide a path where Auto Loader stores the inferred schema: .option("cloudFiles.schemaLocation", "/Volumes/my_catalog/my_schema/checkpoint"). Auto Loader writes the detected schema to this location and reuses it in future micro batches. This prevents schema mismatches and makes ingestion idempotent. Without it, Auto Loader cannot run in streaming mode.
Schema Evolution Modes
The cloudFiles.schemaEvolutionMode option controls what happens when new columns appear in the data. Options are:
- addNewColumns: Adds any new columns to the schema automatically (default)
- rescue: Keeps existing schema, puts unrecognized columns in the _rescued_data column
- failOnNewColumns: Stops processing if new columns appear
- none: Ignores the schema stored at schema location (rarely used)
The Rescue Data Column
When you set cloudFiles.schemaEvolutionMode to rescue mode, Auto Loader adds a hidden column called _rescued_data. Any data that doesn't match the current schema gets serialized as JSON into this column. You can then inspect or parse this column to decide what to do with the mismatched data. This is useful for catching unexpected changes without failing the pipeline.
COPY INTO vs Auto Loader Syntax
COPY INTO is a simple SQL command for loading files into a table in a single operation: COPY INTO my_table FROM 's3://bucket/path' FILEFORMAT = JSON. It's one shot execution, good for batch loads or one time migrations. Auto Loader is for continuous ingestion: it uses spark.readStream.format("cloudFiles") and micro batches new files as they arrive. COPY INTO doesn't support streaming; Auto Loader does.
Trigger Modes
Auto Loader pipelines are streaming, but you control the trigger. trigger(availableNow=True) processes all available files in one batch and then stops (batch like behavior). trigger(processingTime="5 minutes") creates a micro batch every 5 minutes. You can also use trigger(once=True) for once per trigger execution. The trigger doesn't change Auto Loader's ingestion logic, just when it executes.
Schema Hints
You can override or provide schema hints with .option("cloudFiles.schemaHints", "col1 INT, col2 STRING, col3 DOUBLE"). This tells Auto Loader to infer the schema but treat specified columns as the given types. Useful when Auto Loader might infer the wrong type for a column (e.g., inferring a numeric string as LONG instead of STRING).
---
## Code Examples
### Complete Auto Loader Pipeline with Common Options (Python)
```python
from pyspark.sql.types import StructType, StructField, StringType, IntegerType, DoubleType
df = spark.readStream \
  .format("cloudFiles") \
  .option("cloudFiles.format", "json") \
  .option("cloudFiles.schemaLocation", "/Volumes/my_catalog/my_schema/checkpoint") \
  .option("cloudFiles.inferColumnTypes", "true") \
  .option("cloudFiles.schemaHints", "customer_id INT, amount DOUBLE") \
  .load("s3://my-bucket/customer-data/")
df.writeStream \
  .format("delta") \
  .option("checkpointLocation", "/Volumes/my_catalog/my_schema/write_checkpoint") \
  .mode("append") \
  .table("raw_customers")
```
This pipeline reads JSON files from S3, infers column types, stores the schema in a Volumes location, and writes to the raw_customers table in append mode.
### Auto Loader with Schema Evolution and Rescue Column (Python)
```python
df = spark.readStream \
  .format("cloudFiles") \
  .option("cloudFiles.format", "parquet") \
  .option("cloudFiles.schemaLocation", "/Volumes/my_catalog/my_schema/schema_location") \
  .option("cloudFiles.schemaEvolutionMode", "rescue") \
  .option("cloudFiles.inferColumnTypes", "true") \
  .load("s3://data-lake/events/")
# Inspect rescued data to catch unexpected schema changes
df_with_rescued = df.select("*", "_rescued_data")
df_with_rescued.writeStream \
  .format("delta") \
  .option("checkpointLocation", "/Volumes/my_catalog/my_schema/write_ck") \
  .mode("append") \
  .table("events_bronze")
```
This pipeline uses rescue mode, which keeps the existing schema and captures any unrecognized columns in the _rescued_data column. This is useful for handling upstream schema drift without failing the pipeline.
### COPY INTO Syntax (SQL)
```sql
COPY INTO my_schema.raw_data
FROM 's3://my-bucket/archive/2025-01-15/'
FILEFORMAT = JSON
FORMAT_OPTIONS ('inferSchema' = 'true')
COPY_OPTIONS ('mergeSchema' = 'true');
```
COPY INTO is a SQL command for one time or batch loads. It doesn't stream and doesn't require a checkpoint location. It's simpler than Auto Loader for archive or batch operations.
### Auto Loader in Lakeflow Spark Declarative Pipelines (SQL)
```sql
CREATE OR REFRESH STREAMING TABLE raw_orders AS
SELECT *
FROM cloud_files(
  's3://data-lake/orders/',
  'json',
  map(
    'cloudFiles.schemaLocation', '/Volumes/my_catalog/my_schema/orders_schema',
    'cloudFiles.schemaEvolutionMode', 'rescue'
  )
);
```
In Lakeflow Spark Declarative Pipelines, you use the cloud_files() SQL function. It takes the path, format, and options as a map. This is declarative syntax, so Lakeflow manages the pipeline execution and checkpointing automatically.
### Trigger Modes Comparison (Python)
```python
# availableNow: Process all available files once, then stop
df = spark.readStream.format("cloudFiles") \
  .option("cloudFiles.format", "csv") \
  .option("cloudFiles.schemaLocation", "/Volumes/my_catalog/my_schema/ck1") \
  .load("s3://data/csv-files/")
df.writeStream \
  .trigger(availableNow=True) \
  .format("delta") \
  .option("checkpointLocation", "/Volumes/my_catalog/my_schema/write_ck1") \
  .mode("append") \
  .table("csv_data")
# processingTime: Micro batch every 5 minutes
df2 = spark.readStream.format("cloudFiles") \
  .option("cloudFiles.format", "parquet") \
  .option("cloudFiles.schemaLocation", "/Volumes/my_catalog/my_schema/ck2") \
  .load("s3://data/parquet-files/")
df2.writeStream \
  .trigger(processingTime="5 minutes") \
  .format("delta") \
  .option("checkpointLocation", "/Volumes/my_catalog/my_schema/write_ck2") \
  .mode("append") \
  .table("parquet_data")
```
---
## Common Exam Scenarios
A company receives JSON files from a partner every hour. They set up an Auto Loader pipeline but forget to specify the schema location. What happens?
The pipeline fails because schema location is required for streaming Auto Loader. Auto Loader needs to store the inferred schema somewhere persistent so that future micro batches can reuse it and catch schema changes. Without schema location, Auto Loader cannot run in streaming mode. The fix is to add .option("cloudFiles.schemaLocation", "/Volumes/..."). This is a common gotcha because the option name is verbose and easy to overlook.
An engineer receives CSV files with a schema that changes monthly. New columns are added, and they want to load the data without the pipeline failing. Which schema evolution mode should they use?
They should use cloudFiles.schemaEvolutionMode = "rescue". Rescue mode keeps the current schema intact and puts unmatched data into the _rescued_data column. This prevents the pipeline from failing while still preserving the unexpected data for later review and processing. Addnewcolumns mode (the default) would automatically expand the schema, which might not be desired if they want tight schema governance. Failonnewcolumns would break the pipeline, and none would ignore schema evolution completely.
A data engineer wants to load 5 years of historical Parquet files in one batch operation and is not planning to add more files. Should they use Auto Loader or COPY INTO?
They should use COPY INTO. COPY INTO is simpler for one time or batch loads because it doesn't require checkpointing or schema location management. Auto Loader is optimized for continuous streaming ingestion where new files arrive regularly. Using Auto Loader for a one time historical load adds unnecessary complexity (checkpoint overhead, schema location maintenance). COPY INTO executes once, loads all files, and completes. The syntax is simpler: COPY INTO target FROM 's3://path' FILEFORMAT = PARQUET. However, if they later want to switch to continuous ingestion of incoming files, then Auto Loader becomes necessary.
---
## Key Takeaways
- Auto Loader entry point is spark.readStream.format("cloudFiles"). Chain options for format, schema location, and schema evolution mode.
- Schema location is required for streaming. It's where Auto Loader persists the inferred schema. Without it, the pipeline fails immediately.
- Rescue mode captures unrecognized columns in _rescued_data. Use it when you expect schema changes but want the pipeline to stay stable.
- COPY INTO is for batch or one time loads. Auto Loader is for continuous streaming. Don't mix them up.
- Triggers control when the pipeline executes: availableNow=True for batch like processing, processingTime for periodic micro batches.
---
## Gotchas and Tips
Schema Location is Not Optional
Many candidates try to run Auto Loader without specifying cloudFiles.schemaLocation. The pipeline will fail with an error about schema location. You must provide a Volumes path or cloud storage path. This is one of the most common mistakes on the exam.
COPY INTO and Auto Loader Serve Different Use Cases
The exam includes questions about which tool to use for a given scenario. COPY INTO is simpler but doesn't stream. Auto Loader is for continuous ingestion. If the scenario says 'once per week' or 'historical archive', lean toward COPY INTO. If it says 'continuous' or 'as files arrive', choose Auto Loader.
Rescue Mode Requires Understanding of _rescued_data
When you use cloudFiles.schemaEvolutionMode = "rescue", the _rescued_data column automatically appears in your data. Some candidates forget to account for this or don't realize they need to parse it. Know that _rescued_data contains unexpected columns as a JSON string, and you need to handle it downstream.
---
## Links and Resources
- **[What is Auto Loader?](https://docs.databricks.com/aws/en/ingestion/cloud-object-storage/auto-loader)** — Auto Loader overview with syntax examples for cloudFiles format.
- **[Auto Loader options](https://docs.databricks.com/aws/en/ingestion/cloud-object-storage/auto-loader/options)** — Complete reference of all configuration options for Auto Loader.
- **[Configure file notification mode](https://docs.databricks.com/aws/en/ingestion/cloud-object-storage/auto-loader/file-notification-mode)** — Setting up cloud file notifications for scalable ingestion.