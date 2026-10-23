## Topic Overview
The Professional exam expects you to handle real world data coming in multiple formats from diverse sources. You need to know how to read Delta, Parquet, ORC, AVRO, JSON, CSV, XML, text, and binary files from cloud storage and message buses. This is not just about knowing the syntax. You need to understand when to use each format, how to handle schema evolution, and how Auto Loader handles discovery and ingestion at scale.
Auto Loader is the recommended pattern for cloud storage ingestion. It uses cloudFiles to discover new files efficiently, optionally infer schema from files in the source location, and can rescind corrupt or unreadable records to a rescue column instead of failing. Understanding the difference between directory listing (slower, reliable) and notification based discovery (faster, eventually consistent) matters for high throughput pipelines.
Schema inference and evolution are critical in real world pipelines. You need to know how to provide hints to the schema detection, how Spark infers types from file samples, and how to merge new columns as your upstream data changes. The exam tests whether you can build pipelines that adapt to schema changes without breaking.
---
## Key Concepts
File Format Trade offs: Delta vs Parquet vs ORC vs AVRO
Delta is the Databricks standard. It adds transactions, schema enforcement, and time travel on top of Parquet. Use it when you own the table and can enforce schema. Parquet is the industry standard columnar format. It's fast, compressed, and works everywhere. Use Parquet when you need interoperability with non Databricks tools. ORC is columnar like Parquet, slightly smaller files, but less common. AVRO is row based and includes schema in every file, making it good for Kafka or schema evolution scenarios. JSON is human readable but slower and takes more storage. Use JSON when schema is dynamic or you're integrating with APIs. CSV is still common in legacy systems but has edge cases with delimiters and escaping. Binary formats are rare in data pipelines but appear in image, audio, or proprietary data.
Auto Loader Discovery Modes: Directory Listing vs Notifications
Auto Loader uses cloudFiles with two discovery modes. Directory listing (default) checks the folder for new files every trigger. It's simple and reliable but slower for high throughput. Notification mode subscribes to cloud events (S3 EventNotifications, Azure Event Hubs, GCS Pub/Sub). It's faster and cheaper at scale because Auto Loader only processes files that were actually added. The exam tests whether you know when to pick each mode. Use notifications for streaming ingestion at scale. Use directory listing for batch or when notification infrastructure is not available.
Schema Inference and Evolution
Schema inference samples files in the source path and infers types. Spark is conservative and samples a fraction of files, so sometimes types are guessed wrong. You can provide hints via schema option. Schema evolution happens when new columns appear or types change. The mergeSchema option in spark.read accepts new columns. For Lakeflow streaming tables, the cloudFiles.schemaEvolutionMode controls how to handle schema changes: "rescue" adds an extras column for unknown fields, "evolution" merges them as new columns, and "none" fails. The exam asks which mode suits which scenario.
Rescue Columns and Corrupt Records
When Auto Loader encounters a file it cannot parse (corruption, encoding mismatch, schema mismatch), it can fail or rescind the record. The _rescued_data column is a JSON string containing the raw bytes or unparseable fields. You can then inspect, quarantine, or manually fix these records. Setting cloudFiles.inferColumnTypes helps detect type issues early. The key question is: do you want the pipeline to fail loudly or gracefully handle bad data? The Professional exam expects you to choose the right mode.
---
## Code Examples
### Reading Different File Formats with spark.read
```python
# Delta
df_delta = spark.read.format("delta").load("/path/to/table")
# Parquet
df_parquet = spark.read.parquet("/path/to/file.parquet")
# ORC
df_orc = spark.read.orc("/path/to/file.orc")
# AVRO with schema registry
df_avro = spark.read.format("avro").load("/path/to/file.avro")
# JSON with schema inference
df_json = spark.read.json("/path/to/file.json")
# CSV with explicit schema
schema = "id INT, name STRING, age INT"
df_csv = spark.read.schema(schema).csv("/path/to/file.csv", header=True)
# Multiple files with same format
df_multi = spark.read.parquet("/path/to/dir/*.parquet")
```
### Auto Loader cloudFiles Syntax
```python
# Basic Auto Loader with directory listing
df = spark.readStream \
  .format("cloudFiles") \
  .option("cloudFiles.format", "parquet") \
  .load("/Volumes/my_catalog/my_schema/my_volume/data/")
# Auto Loader with notifications (AWS S3)
df = spark.readStream \
  .format("cloudFiles") \
  .option("cloudFiles.format", "parquet") \
  .option("cloudFiles.useNotifications", "true") \
  .option("cloudFiles.subscriptionId", "arn:aws:sqs:....") \
  .load("s3://my-bucket/data/")
# Auto Loader with schema hints
schema = "id INT, name STRING, created_at TIMESTAMP"
df = spark.readStream \
  .format("cloudFiles") \
  .option("cloudFiles.format", "json") \
  .option("cloudFiles.schemaLocation", "/Volumes/schemas/") \
  .schema(schema) \
  .load("/Volumes/my_catalog/my_schema/my_volume/json_data/")
# Auto Loader with schema evolution
df = spark.readStream \
  .format("cloudFiles") \
  .option("cloudFiles.format", "json") \
  .option("cloudFiles.schemaEvolutionMode", "rescue") \
  .load("/Volumes/my_catalog/my_schema/my_volume/data/")
```
### Handling Schema Evolution in Lakeflow Pipelines
```python
from databricks.sdk.python import DatabricksSession
# Lakeflow streaming table with rescue mode
df = dlt.create_streaming_table(
  name="raw_events",
  comment="Events from Event Hub with rescue column"
)
df = spark.readStream \
  .format("cloudFiles") \
  .option("cloudFiles.format", "json") \
  .option("cloudFiles.schemaEvolutionMode", "rescue") \
  .load("/Volumes/my_catalog/my_schema/my_volume/events/")
dlt.append_or_replace_table(
  name="raw_events",
  spark_df=df,
  expect_all_or_drop={"valid_json": "_rescued_data IS NULL"}
)
# Read the _rescued_data for debugging
df_with_rescue = spark.readStream \
  .format("cloudFiles") \
  .option("cloudFiles.format", "json") \
  .option("cloudFiles.schemaEvolutionMode", "rescue") \
  .load("/path/to/data/")
# Filter bad records
df_good = df_with_rescue.filter("_rescued_data IS NULL")
df_bad = df_with_rescue.filter("_rescued_data IS NOT NULL").select("_rescued_data")
```
### mergeSchema for Dynamic Schema
```python
# Read with schema merge enabled
df = spark.read \
  .option("mergeSchema", "true") \
  .parquet("/path/to/parquet/dir/")
# For JSON, similar approach
df = spark.read \
  .option("mergeSchema", "true") \
  .json("/path/to/json/dir/")
# In streaming context with Auto Loader
df = spark.readStream \
  .format("cloudFiles") \
  .option("cloudFiles.format", "parquet") \
  .option("cloudFiles.schemaEvolutionMode", "evolution") \
  .load("/Volumes/my_catalog/my_schema/my_volume/data/")
```
---
## Common Exam Scenarios
Scenario 1: Schema Evolution Challenge
You are building a real time pipeline that ingests events from a data lake. The upstream team occasionally adds new fields to the JSON payloads without warning. Your pipeline crashes when it encounters the new fields. Which schemaEvolutionMode should you use, and why?
Use "rescue" mode. This captures unknown fields in the _rescued_data column instead of failing the pipeline. You can then decide whether to merge those fields later or quarantine records with unexpected schema. "evolution" mode would add new columns automatically, which might cause downstream issues if you have strict schema enforcement. "none" mode fails the pipeline entirely.
Scenario 2: Reducing Cloud Storage Costs
Your team ingests millions of files daily from S3. Your current Auto Loader setup uses directory listing and polls every 5 minutes, but you are paying high bills for ListObject API calls. The S3 bucket supports EventNotifications. How can you reduce costs?
Switch to notification mode. Set cloudFiles.useNotifications to true and provide the S3 EventNotifications queue ARN via cloudFiles.subscriptionId. Auto Loader will subscribe to events instead of polling. This eliminates the ListObject API calls and is much cheaper at scale. You still get reliable delivery because notifications are eventually consistent.
Scenario 3: Handling Corrupt Data
You are reading CSV files with mixed encodings (some UTF 8, some Latin 1). Some files have malformed rows. You want to extract the malformed rows for inspection without failing the entire job. How should you handle this?
Use Auto Loader with cloudFiles.schemaEvolutionMode set to "rescue". Provide a schema hint so Spark knows the expected structure. When it encounters malformed rows, they go to _rescued_data. Then filter and save records where _rescued_data IS NOT NULL to a quarantine table for manual review. This pattern keeps your main pipeline clean while capturing errors for debugging.
---
## Key Takeaways
- Auto Loader with cloudFiles is the modern, recommended pattern for cloud storage ingestion. It handles discovery, schema inference, and error handling elegantly.
- Use notification mode for high throughput at scale. Directory listing is fine for lower volume or when notifications are not available.
- schemaEvolutionMode="rescue" is your friend for real world pipelines. It lets you handle schema changes gracefully without failing the job.
- Always provide schema hints when possible. Spark's inference is conservative and can guess wrong if it only samples a few files.
- _rescued_data is a JSON string of unparseable fields. Use it to build quarantine tables for bad records, not as a production column.
---
## Gotchas and Tips
Notification mode is eventually consistent. Events can be delayed or arrive out of order. If you need strict ordering, stick with directory listing or add your own sorting logic downstream.
schemaEvolutionMode="evolution" adds new columns automatically, which can break downstream transformations that expect a fixed schema. Use "rescue" if you want to review before merging.
Parquet files with mergeSchema=true can be slow on large directories. Spark reads all files to infer the union of schemas. Provide explicit schemas when possible.
---
## Links and Resources
- **[Auto Loader](https://docs.databricks.com/aws/en/ingestion/cloud-object-storage/auto-loader)** — Incrementally ingesting files from cloud storage in diverse formats (JSON, CSV, Avro, Parquet).
- **[Streaming with Delta Lake](https://docs.databricks.com/aws/en/structured-streaming/delta-lake)** — Using Delta Lake as a streaming source and sink for diverse data formats.