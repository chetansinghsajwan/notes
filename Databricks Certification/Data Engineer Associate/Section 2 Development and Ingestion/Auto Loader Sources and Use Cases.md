## Topic Overview
Auto Loader is a Databricks feature that automatically detects and ingests new data files as they arrive in cloud storage. It works with S3, ADLS Gen2, GCS, and Unity Catalog Volumes, making it ideal for building scalable data pipelines that can handle thousands or millions of files without manual intervention. The feature uses checkpointing to track processed files and guarantees exactly once processing semantics, which is critical for data pipelines where duplicate data could cause downstream issues.
What makes Auto Loader powerful is its ability to work in two distinct modes for discovering new files: directory listing (the default, which simply scans the directory) and file notification (which leverages cloud service events for real time discovery at scale). The choice between these modes depends on your data volume, latency requirements, and the performance characteristics of your cloud storage. Auto Loader handles schema inference and evolution automatically, so your pipelines can adapt as the structure of incoming data changes over time.
On the exam, you'll need to know which file formats Auto Loader supports (JSON, CSV, Parquet, Avro, ORC, Text, Binary, XML), when to use Auto Loader versus other ingestion methods like COPY INTO, and how to configure it for both batch and streaming scenarios. Auto Loader typically feeds into the Bronze layer of a Medallion Architecture pipeline, serving as the entry point for raw data.
---
## Key Concepts
**What is Auto Loader and why use it?**
Auto Loader is a streaming source that automatically discovers and ingests new data files from cloud storage. Instead of manually checking for new files or writing complex orchestration logic, Auto Loader handles file discovery and incremental loading out of the box. It uses the `cloudFiles` format and maintains a checkpoint (RocksDB based) to remember which files have already been processed. This checkpoint ensures that files are processed exactly once, preventing duplicates. Auto Loader works in both batch and streaming modes, giving you flexibility in how you consume data.
**Directory listing vs file notification mode**
Directory listing (default) scans the entire directory to find new files. This works well for small to medium volumes but becomes slow as file counts grow into the millions. File notification mode leverages cloud service events (S3 EventBridge, ADLS notifications, GCS pub/sub) to detect new files in real time, which is far more efficient at scale and provides lower latency. Directory listing has no additional setup and works everywhere; file notification requires cloud service configuration but scales much better. For the exam, know that file notification is the recommended mode for high volume scenarios.
**Supported file formats**
Auto Loader supports a wide range of file formats: JSON, CSV, Parquet, Avro, ORC, Text, Binary, and XML. You specify the format using the `format` parameter when reading. Different formats have different trade offs. Parquet is efficient for analytical queries; JSON and CSV are human readable but larger; Avro and ORC are good for streaming; Binary is for unstructured data. On the exam, you should be able to recommend the right format for a given scenario.
**Schema inference and evolution**
Auto Loader automatically infers the schema from incoming files. If you don't provide a schema hint, it samples the data to figure out column names and types. As new files arrive with additional columns or different types, Auto Loader can evolve the schema to accommodate them (if you enable the `schemaEvolutionMode` parameter). This is powerful for pipelines where the source data format changes over time. However, on the exam you should know that providing an explicit schema hint is a best practice for production pipelines, as it gives you control and prevents unexpected schema changes.
**Checkpointing and exactly once guarantees**
Auto Loader stores a checkpoint that tracks which files have been processed. This checkpoint is RocksDB based and lives in the location specified by the `cloudFiles.useNotifications` and related options. The checkpoint guarantees exactly once processing: each file is ingested exactly once, even if a job fails and restarts. This is essential for data quality. If you need to reprocess data, you can reset the checkpoint, but normally you should not touch it. Checkpoint corruption is rare but can happen; for the exam, know that checkpoints exist and serve this critical function.
---
## Code Examples
### Basic Auto Loader Streaming Read (PySpark)
```python
# Basic Auto Loader read in streaming mode
df = spark.readStream \
  .format("cloudFiles") \
  .option("cloudFiles.format", "json") \
  .option("cloudFiles.schemaLocation", "/Volumes/catalog/schema/") \
  .load("s3://my-bucket/data/") 
df.writeStream \
  .mode("append") \
  .option("checkpointLocation", "/Volumes/catalog/checkpoint/") \
  .table("bronze_events")
```
The `cloudFiles.schemaLocation` parameter tells Auto Loader where to store the inferred schema. The checkpoint location tells Spark where to maintain state for the streaming job.
### Auto Loader with Schema Hints (PySpark)
```python
from pyspark.sql.types import StructType, StructField, StringType, IntegerType, TimestampType
# Define explicit schema
schema = StructType([
  StructField("id", StringType()),
  StructField("name", StringType()),
  StructField("age", IntegerType()),
  StructField("timestamp", TimestampType())
])
# Read with explicit schema
df = spark.readStream \
  .format("cloudFiles") \
  .option("cloudFiles.format", "json") \
  .schema(schema) \
  .load("s3://my-bucket/raw-data/")
df.writeStream \
  .mode("append") \
  .option("checkpointLocation", "/tmp/checkpoint/") \
  .table("bronze_customers")
```
Providing an explicit schema is a best practice in production. It makes your pipeline more predictable and catches schema mismatches early rather than at runtime.
### Auto Loader in Lakeflow Declarative Pipelines (SQL)
```sql
-- Create a streaming table from Auto Loader in a Lakeflow pipeline
CREATE STREAMING TABLE bronze_logs
COMMENT "Raw logs ingested via Auto Loader"
AS
SELECT 
  *,
  _metadata.file_path,
  _metadata.file_modification_time
FROM read_files(
  "s3://my-bucket/logs/",
  format => "json",
  cloudFiles => true
)
```
In Lakeflow Spark Declarative Pipelines (also called Delta Live Tables in legacy documentation), the `read_files` function with `cloudFiles => true` activates Auto Loader. The `_metadata` columns give you access to file path and modification time, useful for tracking data lineage.
---
## Common Exam Scenarios
**Scenario 1: Auto Loader vs COPY INTO**
Your team is building a data pipeline to ingest millions of new JSON files from S3 every hour. The files arrive at unpredictable intervals. Which ingestion method should you use? Auto Loader is the clear choice here. COPY INTO is a one time bulk load command; it doesn't automatically detect new files. Auto Loader continuously monitors the directory and ingests new files as they arrive. If the exam gives you a scenario about periodic, automatic ingestion of new files, Auto Loader is your answer.
**Scenario 2: Choosing between directory listing and file notification**
Your pipeline currently ingests 50,000 files per day using Auto Loader's directory listing mode. The data team reports that the time from file upload to processing is increasing as the number of files grows. You need to lower latency and reduce API calls to cloud storage. The solution is to switch to file notification mode. File notification uses cloud service events (S3 EventBridge, ADLS notifications) which are triggered immediately when files land. This is far more efficient than scanning the entire directory every time. The trade off is that you need to set up cloud service event notifications, but this is well worth it for large file volumes.
**Scenario 3: Handling schema changes in incoming files**
Your Bronze layer ingests customer data from multiple sources. The schema of incoming files occasionally changes when upstream systems add new fields. You want your pipeline to continue working without manual intervention. Auto Loader with schema evolution enabled will automatically accommodate new columns. However, the exam may ask what the best practice is. The answer is to provide an explicit schema hint using the `schema()` parameter, combined with `schemaEvolutionMode` set to "addNewColumns". This gives you control over schema changes rather than blindly accepting anything Auto Loader infers. This approach catches unexpected changes early and logs them for review.
---
## Key Takeaways
- Auto Loader automatically discovers and ingests new files from cloud storage (S3, ADLS Gen2, GCS, Unity Catalog Volumes) with exactly once processing guarantees via checkpointing.
- Directory listing mode scans the directory each time; file notification mode uses cloud events and scales much better for large file volumes.
- Auto Loader supports JSON, CSV, Parquet, Avro, ORC, Text, Binary, and XML formats with automatic schema inference and optional schema evolution.
- Best practice: provide an explicit schema hint using the `schema()` parameter in production for predictable, controlled ingestion pipelines.
- Auto Loader works in both batch and streaming modes and feeds data into the Bronze layer of a Medallion Architecture pipeline.
---
## Gotchas and Tips
**Checkpoint Corruption Risk: Never manually delete or move the checkpoint directory. Corruption can cause files to be reprocessed or skipped entirely.**
**File Notification Setup Required: File notification mode is powerful but requires upfront cloud service setup. For S3, you need EventBridge; for ADLS, you configure event grid subscriptions. This is a common exam gotcha.**
**Schema Evolution Can Hide Bugs: Enabling schema evolution without review means unexpected changes go undetected. Always combine schema evolution with monitoring and explicit schema hints for production pipelines.**
---
## Links and Resources
- **[What is Auto Loader?](https://docs.databricks.com/aws/en/ingestion/cloud-object-storage/auto-loader)** — Overview of Auto Loader for incrementally ingesting files from cloud storage.
- **[Compare Auto Loader file detection modes](https://docs.databricks.com/aws/en/ingestion/cloud-object-storage/auto-loader/file-detection-modes)** — Directory listing vs file notification mode comparison.
- **[Configure Auto Loader for production](https://docs.databricks.com/aws/en/ingestion/cloud-object-storage/auto-loader/production)** — Best practices for running Auto Loader in production workloads.