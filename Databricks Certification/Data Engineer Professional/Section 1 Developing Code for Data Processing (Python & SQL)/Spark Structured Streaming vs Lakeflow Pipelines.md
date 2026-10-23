## Topic Overview
Spark Structured Streaming and Lakeflow Spark Declarative Pipelines are two approaches to streaming data processing on Databricks. Spark Structured Streaming gives you complete control over micro batch execution, checkpointing, and stateful transformations. You write code in PySpark or Scala using readStream and writeStream APIs, manage your own state backends, and handle error recovery manually. Lakeflow Spark Declarative Pipelines offer a higher level abstraction where you declare data sources, transformations, and sinks, and Lakeflow manages the underlying streaming mechanics for you.
The choice between the two depends on your requirements. Use Spark Structured Streaming when you need fine grained control, custom trigger modes, or stateful operations that Lakeflow does not yet support. Use Lakeflow when you prioritize simplicity, built in error handling, automatic state management, and integration with Delta Lake's governance features like quality expectations and change data feed.
On the exam, you will see scenarios asking whether to use raw Structured Streaming or Lakeflow Pipelines for a given use case, how each handles state management and recovery, and how to monitor each approach. Understanding the trade offs and knowing when to reach for each tool is essential.
---
## Key Concepts
**Structured Streaming: Manual State and Checkpointing**
In Structured Streaming, you explicitly manage state using stateful operations like mapGroupsWithState or flatMapGroupsWithState. You define checkpoint directories and recovery behavior. Lakeflow abstracts this away; state is managed transparently and checkpoints are automatic.
**Trigger Modes and Execution Semantics**
Spark Structured Streaming supports trigger modes: default (micro batch ASAP), fixed interval, one time, and continuous. Lakeflow uses fixed interval triggers (you set the refresh rate). If you need continuous or event driven triggers, Structured Streaming is required. For standard batch processing with guaranteed once semantics, both work.
**Declarative vs Imperative Approach**
Lakeflow uses a declarative approach: you say 'what' you want (create a streaming table from Kafka, apply this transformation, land in Delta). Structured Streaming is imperative: you say 'how' to fetch, transform, and write. Declarative is simpler to maintain; imperative gives you more control.
**Streaming Table vs Materialized View**
Lakeflow streaming tables are continuously refreshed views backed by Delta, ideal for CDC and append only ingestion. Materialized views compute once and cache results. Understand when to use each in Lakeflow pipelines, and contrast with how Structured Streaming handles these scenarios.
---
## Code Examples
### Spark Structured Streaming: readStream and writeStream
```python
from pyspark.sql.functions import col, window, count
df = spark.readStream \
    .format("kafka") \
    .option("kafka.bootstrap.servers", "localhost:9092") \
    .option("subscribe", "events") \
    .option("startingOffsets", "earliest") \
    .load()
df = df.select(
    col("timestamp"),
    col("value").cast("string").alias("event")
)
# Stateful transformation with window
agg_df = df.groupBy(
    window(col("timestamp"), "1 minute"),
    col("event")
).count()
# Write with checkpoint for recovery
agg_df.writeStream \
    .format("delta") \
    .option("checkpointLocation", "/checkpoints/events_agg") \
    .option("mergeSchema", "true") \
    .outputMode("append") \
    .start("/warehouse/events_agg")
```
With Structured Streaming, you explicitly define the checkpoint location for recovery. If the stream fails, it resumes from the checkpoint. You control trigger behavior (default micro batch), output mode (append, update, complete), and stateful operations.
### Equivalent Lakeflow Spark Declarative Pipeline
```python
import dlt
from pyspark.sql.functions import col, window, count
@dlt.table
def events_stream():
    return spark.readStream \
        .format("cloudFiles") \
        .option("cloudFiles.format", "json") \
        .option("cloudFiles.schemaLocation", "/schema_checkpoint") \
        .load("/source/events")
@dlt.table
def events_agg():
    return dlt.read("events_stream") \
        .groupBy(
            window(col("timestamp"), "1 minute"),
            col("event")
        ).count()
```
With Lakeflow, you simply declare the source and transformations. Lakeflow manages checkpointing, state, and recovery automatically. The code is shorter and less error prone. Streaming tables are automatically backed by Delta with quality expectations support.
### Trigger Options in Structured Streaming
```python
# Micro batch ASAP (default)
query1 = df.writeStream \
    .option("checkpointLocation", "/cp1") \
    .format("delta") \
    .start("/table1")
# Fixed interval (every 30 seconds)
query2 = df.writeStream \
    .trigger(processingTime="30 seconds") \
    .option("checkpointLocation", "/cp2") \
    .format("delta") \
    .start("/table2")
# One time (batch mode)
query3 = df.writeStream \
    .trigger(once=True) \
    .option("checkpointLocation", "/cp3") \
    .format("delta") \
    .start("/table3")
# Continuous (experimental)
query4 = df.writeStream \
    .trigger(continuous="1 second") \
    .option("checkpointLocation", "/cp4") \
    .format("delta") \
    .start("/table4")
```
Structured Streaming offers flexible trigger modes. Lakeflow defaults to fixed interval triggers with configurable refresh rate in the job config. If you need continuous or one time triggers, Structured Streaming is the better choice.
---
## Common Exam Scenarios
**Scenario 1: Real time event aggregation with CDC tracking**
You need to continuously ingest customer events from Kafka, aggregate them into hourly windows, and maintain a complete audit trail of dimension changes (e.g., customer tier upgrades). Lakeflow is the right choice because you can define streaming tables for raw events, apply APPLY CHANGES for dimension updates, and use expectations to validate quality. Structured Streaming would require you to manually manage state for both the aggregate and the slowly changing dimension.
**Scenario 2: Custom stateful processing with sessionization**
Your company wants to sessionize user behavior: group events by user into sessions separated by 30 minutes of inactivity. You need to use mapGroupsWithState to track session state across micro batches. Lakeflow does not directly expose this API, so Structured Streaming is required. You would use a Structured Streaming job and write results to Delta for downstream consumption.
**Scenario 3: Batch reconciliation with one time trigger**
You want to recompute historical aggregates one time as a batch job, triggered manually from a Lakeflow Job. Use Structured Streaming with trigger(once=True) to ensure it processes all pending data in the stream and stops. This is faster and more efficient than running a pure batch job, and it respects the streaming architecture.
---
## Key Takeaways
- Lakeflow Spark Declarative Pipelines simplify streaming pipelines with declarative syntax and automatic state/checkpoint management
- Spark Structured Streaming provides fine grained control over triggers, state management, and output modes for complex streaming logic
- Use Lakeflow for standard ingestion, CDC, and aggregation scenarios; use Structured Streaming for custom stateful operations and trigger modes
- Both approaches write to Delta and support checkpointing, but Lakeflow integrates with quality expectations and governance features
- Structured Streaming trigger modes include micro batch (default), fixed interval, one time (batch), and continuous (experimental)
---
## Gotchas and Tips
⚠️ Checkpoint location matters: In Structured Streaming, an incorrect or missing checkpoint location causes the query to reprocess old data or fail on restart. Always set a unique, durable checkpoint path.
⚠️ Output mode is critical: Using outputMode('complete') with large datasets is expensive. Use 'append' for insert only and 'update' for aggregations. Lakeflow defaults to 'append' for streaming tables.
⚠️ Lakeflow state is opaque: You cannot inspect Lakeflow's internal state like you can with Structured Streaming. If you need visibility into session state or custom counters, Structured Streaming is more appropriate.
---
## Links and Resources
- **[Structured Streaming Tutorial](https://docs.databricks.com/aws/en/structured-streaming/tutorial)** — Getting started with Spark Structured Streaming on Databricks.
- **[Lakeflow Spark Declarative Pipelines](https://docs.databricks.com/aws/en/ldp/)** — Comparing declarative pipeline development vs manual Structured Streaming.
- **[Streaming with Delta Lake](https://docs.databricks.com/aws/en/structured-streaming/delta-lake)** — Using Delta Lake as a streaming source and sink.