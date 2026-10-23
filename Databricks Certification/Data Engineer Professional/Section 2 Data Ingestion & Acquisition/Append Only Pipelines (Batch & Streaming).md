## Topic Overview
Append only pipelines are a fundamental pattern in data lakes. You ingest data once, and it flows into Bronze, Silver, and Gold tables without being modified or deleted. This pattern guarantees immutability and simplifies debugging because you can always replay history.
The challenge is ensuring idempotency. If your job fails and reruns, you must not duplicate records. Batch append uses INSERT INTO with deduplication logic. Streaming append uses Auto Loader with exactly once semantics via Lakeflow pipelines. The Professional exam expects you to understand the trade offs between batch and streaming, and when to combine them in a single pipeline architecture.
Append only is not just a technical pattern. It is a data governance best practice. It ensures an auditable history and makes compliance easier because you never lose data. The key is building deduplication and exactly once semantics into your pipeline from the start.
---
## Key Concepts
Idempotent Writes and Exactly Once Semantics
Idempotency means running the same operation twice produces the same result as running it once. In append only pipelines, you need to guarantee that retries do not create duplicates. Exactly once semantics (EOS) means every record is delivered and processed exactly once, no more, no less. Delta Lake provides this via transactions and checkpointing. When using Auto Loader in Lakeflow, checkpoints automatically track which files have been processed. If the job restarts, it resumes from the checkpoint rather than reprocessing the same files.
Batch Append vs Streaming Append
Batch append processes a fixed set of files in one shot. You run INSERT INTO with a WHERE clause to deduplicate by a key (usually file name or timestamp). It is simple and predictable. Streaming append processes new files continuously as they arrive. Auto Loader discovers files and Auto Loader generates micro batches. Streaming is lower latency but requires more operational overhead. Choose batch for daily or weekly ingestion. Choose streaming for real time requirements.
COPY INTO for Idempotent Batch Loads
COPY INTO is a Databricks SQL command designed for idempotent batch ingestion. It reads files from a source path and inserts only new files into the target table. It keeps an internal state of which files have been processed. If you run the same COPY INTO command twice, the second run does nothing. This is simpler than manually deduplicating with INSERT INTO WHERE.
Checkpointing in Lakeflow Pipelines
Lakeflow streaming tables automatically maintain checkpoints in a hidden directory under the Lakeflow location. The checkpoint tracks which files Auto Loader has already processed. If the pipeline restarts, it reads the checkpoint and skips already processed files. This guarantees no duplicates even if the job crashes mid execution. You do not configure checkpoints manually. They are automatic.
---
## Code Examples
### Batch Append with INSERT INTO and Deduplication
```sql
-- Batch append with deduplication by file name
INSERT INTO my_catalog.my_schema.raw_events
SELECT * 
FROM read_files(
  '/Volumes/my_catalog/my_schema/my_volume/events/',
  format => 'json'
)
WHERE file_path NOT IN (
  SELECT DISTINCT _metadata.file_path FROM my_catalog.my_schema.raw_events
);
-- Or use a timestamp based approach
INSERT INTO my_catalog.my_schema.raw_events
SELECT * 
FROM external_table_path
WHERE ingestion_date = CURRENT_DATE()
AND NOT EXISTS (
  SELECT 1 FROM my_catalog.my_schema.raw_events re
  WHERE re.event_id = external_table_path.event_id
  AND re.ingestion_date = CURRENT_DATE()
);
```
### COPY INTO for Idempotent Batch Loads
```sql
-- COPY INTO automatically tracks processed files
COPY INTO my_catalog.my_schema.raw_events
FROM 's3://my-bucket/data/events/'
FILEFORMAT = JSON
FORMAT_OPTIONS ('inferSchema' = 'true')
COPY_OPTIONS ('mergeSchema' = 'true');
-- Run this command multiple times safely. Only new files are added.
-- COPY INTO maintains internal state of processed files
-- For CSV with specific schema
COPY INTO my_catalog.my_schema.customers
FROM '/Volumes/my_catalog/my_schema/my_volume/customers/'
FILEFORMAT = CSV
FORMAT_OPTIONS ('header' = 'true', 'delimiter' = ',')
COLUMN_MAPPING = {
  'customer_id' : 'customer_id',
  'name' : 'customer_name',
  'email' : 'email_address'
};
```
### Streaming Append with Auto Loader and Lakeflow
```python
# Lakeflow streaming table with Auto Loader
# Automatically handles exactly once semantics via checkpoints
df = dlt.create_streaming_table(
  name="raw_events",
  comment="Real time events from message bus"
)
df = spark.readStream \
  .format("cloudFiles") \
  .option("cloudFiles.format", "json") \
  .option("cloudFiles.schemaEvolutionMode", "rescue") \
  .load("/Volumes/my_catalog/my_schema/my_volume/events/")
# Append to target table
dlt.append_or_replace_table(
  name="raw_events",
  spark_df=df
)
# Bronze to Silver transformation (also streaming and append only)
df_silver = dlt.read_stream("raw_events") \
  .filter("_rescued_data IS NULL") \
  .select("event_id", "event_name", "timestamp", "user_id")
dlt.create_streaming_table(name="silver_events")
dlt.append_or_replace_table(
  name="silver_events",
  spark_df=df_silver
)
```
### Combining Batch and Streaming in One Pipeline
```python
# Scenario: Ingest historical data via batch, real time data via streaming
# Batch: Catch up historical events from S3 snapshot
df_historical = spark.read.format("parquet").load("s3://my-bucket/historical/events/")
df_historical = df_historical.withColumn("_ingestion_type", lit("batch"))
df_historical.write.format("delta").mode("append").saveAsTable("my_catalog.my_schema.all_events")
# Streaming: Real time events from message bus
df_realtime = spark.readStream \
  .format("cloudFiles") \
  .option("cloudFiles.format", "json") \
  .load("/Volumes/my_catalog/my_schema/my_volume/realtime_events/")
df_realtime = df_realtime.withColumn("_ingestion_type", lit("stream"))
# Both feed the same table
df_realtime.writeStream \
  .format("delta") \
  .mode("append") \
  .option("checkpointLocation", "/Volumes/my_catalog/my_schema/my_volume/checkpoints/realtime") \
  .toTable("my_catalog.my_schema.all_events")
# Query sees unified view of both batch and streaming data
df_all = spark.read.table("my_catalog.my_schema.all_events")
```
---
## Common Exam Scenarios
Scenario 1: Batch Append with Duplicates
You run a daily batch job that loads customer records from a CRM export. Yesterday the job failed midway and you restarted it. Today you run the same job again on new data. How do you avoid loading duplicate records from yesterday?
Use COPY INTO instead of INSERT INTO. COPY INTO tracks which files it has already processed and skips them on retry. Alternatively, use INSERT INTO with a WHERE clause that checks against a file manifest table or deduplicates by a key like customer_id + last_modified_date. Always include a deduplication step in your batch logic.
Scenario 2: Streaming Pipeline Crash and Recovery
Your Lakeflow streaming pipeline processes events from a message bus. The cluster crashed while processing a batch. You restart the job. Do you risk reprocessing the same batch twice?
No. Lakeflow maintains automatic checkpoints that track the last successfully processed offset or file. When the job restarts, it reads the checkpoint and resumes from that point, not from the beginning. This guarantees exactly once semantics. You do not need to configure anything. It is built in.
Scenario 3: Combining Historical and Real Time Data
You need to load 10 years of historical events from a data warehouse export, then continuously ingest new events from a Kafka topic into the same table. How do you design this?
Use batch COPY INTO or INSERT INTO for historical data (run once). Then set up a separate streaming Lakeflow pipeline for real time events. Both write to the same target table in append mode. Lakeflow handles the checkpoint automatically so real time data starts from where historical left off. Both paths write to the same table without conflicts because they append only.
---
## Key Takeaways
- Append only is a governance best practice. It creates an immutable audit trail and simplifies replay and debugging.
- Batch append uses COPY INTO for automatic idempotency or INSERT INTO with manual deduplication logic.
- Streaming append with Lakeflow auto Loader guarantees exactly once semantics via automatic checkpointing.
- You can combine batch and streaming writes to the same table safely. Both use append mode and do not conflict.
- Always design idempotency from the start. Retries and restarts are inevitable in production.
---
## Gotchas and Tips
COPY INTO is only idempotent within the same source directory. If you move files or change the source path, COPY INTO may reprocess them. Use file naming conventions (dates, UUIDs) to avoid this.
Checkpoints are stored in the Lakeflow location. Do not delete checkpoints manually or your streaming pipeline will restart from the beginning and duplicate data.
Append only does not mean no deletes. You can still use time travel or maintain a separate "tombstone" table for logical deletes. But the Bronze table itself should never have UPDATE or DELETE operations.
---
## Links and Resources
- **[Streaming Tables](https://docs.databricks.com/aws/en/ldp/streaming-tables)** — Building append-only streaming pipelines for batch and streaming ingestion.
- **[Auto Loader](https://docs.databricks.com/aws/en/ingestion/cloud-object-storage/auto-loader)** — Incrementally loading new files as they arrive in cloud storage.