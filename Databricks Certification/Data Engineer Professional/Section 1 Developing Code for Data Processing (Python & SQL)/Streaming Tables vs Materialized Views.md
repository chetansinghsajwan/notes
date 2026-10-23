## Topic Overview
Streaming tables and materialized views are two different approaches to maintaining computed results in Lakeflow pipelines. Both are defined using the @dlt.table decorator, but they have different refresh semantics and are optimized for different use cases. Understanding when to use each is crucial for building efficient pipelines.
Streaming tables are designed for append only data. They process each input record exactly once, maintain state about what has been processed, and produce append only output. This is ideal for real time or near real time workloads where data arrives continuously. Materialized views, on the other hand, are recomputed from scratch (or incrementally if possible) every time the pipeline runs. They support arbitrary transformations including aggregations and joins, but are more expensive to compute.
For the Professional exam, you need to understand the syntax differences, when to choose each type, the performance implications, and how they behave in a Lakeflow pipeline. The exam may test your ability to recognize which data flow patterns require streaming tables vs materialized views.
---
## Key Concepts
Streaming Tables
Streaming tables process data continuously and maintain state about what has been ingested. Each record is processed exactly once, making them ideal for append only data sources like Kafka topics or Auto Loader directories. Streaming tables output is always append only. Because they maintain state, they are efficient for continuous ingestion patterns. Use streaming tables when you have a continuous stream of events or data and want to process them in real time or near real time without recomputing the entire dataset.
Materialized Views
Materialized views compute results from their dependencies and materialize (store) the results. Every time a pipeline run executes, the materialized view is recomputed based on current data. This allows for arbitrary transformations including aggregations, joins, windowing, and filtering. The downside is that recomputation can be expensive. Use materialized views when you need to support complex transformations or when data arrives in batches and you want results recomputed periodically.
Refresh Semantics
Streaming tables refresh incrementally by processing only new input records. Materialized views refresh by recomputing from their source tables. If a materialized view depends on a streaming table, the materialized view processes only the new records appended since the last run. If a materialized view depends on another materialized view, it recomputes based on the current state of that parent. This distinction matters for performance: streaming tables are more efficient for incremental processing.
Stateful vs Stateless Operations
Streaming tables maintain state between runs (e.g., what records have been seen). This allows them to track incremental progress and avoid reprocessing. Materialized views are stateless; they recompute from scratch or from current source state. Stateful operations like deduplication, exactly once semantics, and state dependent joins are natural for streaming tables. Stateless operations like filtering, mapping, and simple aggregations work well with both.
---
## Code Examples
### Streaming Table with Auto Loader
```python
import dlt
from pyspark.sql.functions import col, from_json, schema_of_json
# Streaming table that ingests JSON events continuously
@dlt.table(
    name="raw_events",
    comment="Raw event stream from Auto Loader"
)
def raw_events():
    # Auto Loader processes new files as they arrive
    return dlt.read_stream("cloud_files") \
        .option("cloudFiles.format", "json") \
        .option("cloudFiles.schemaLocation", "/tmp/schema") \
        .load("/Volumes/data/events")
# Materialized view that processes streaming table (supports aggregations)
@dlt.table(
    name="events_by_type",
    comment="Event counts by type (materialized view)"
)
def events_by_type():
    return dlt.read("raw_events") \
        .groupBy("event_type") \
        .count() \
        .alias("event_count")
```
### Streaming Table for Deduplication
```python
import dlt
from pyspark.sql.functions import col, row_number, desc
from pyspark.sql.window import Window
# Streaming table that deduplicates on event_id
@dlt.table(
    name="deduplicated_events",
    comment="Events deduplicated by ID, keeping latest"
)
def deduplicated_events():
    events = dlt.read_stream("raw_events")
    
    # Window function to rank by timestamp within each ID
    window_spec = Window.partitionBy("event_id").orderBy(desc("timestamp"))
    
    # Keep only the latest record for each event_id
    return events \
        .withColumn("rn", row_number().over(window_spec)) \
        .filter(col("rn") == 1) \
        .drop("rn")
```
### Materialized View for Complex Aggregation
```sql
-- Materialized view computed from multiple sources
CREATE OR REPLACE TABLE dlt.hourly_metrics AS
SELECT 
    DATE_TRUNC('hour', event_timestamp) AS hour,
    event_type,
    COUNT(*) AS event_count,
    COUNT(DISTINCT user_id) AS unique_users,
    PERCENTILE(duration_ms, 0.5) AS median_duration,
    MAX(duration_ms) AS max_duration
FROM dlt.deduplicated_events
WHERE event_timestamp >= CURRENT_TIMESTAMP() - INTERVAL 24 HOURS
GROUP BY 
    DATE_TRUNC('hour', event_timestamp),
    event_type
```
---
## Common Exam Scenarios
An ingestion pipeline receives JSON events from a Kafka topic continuously. Each event has a unique event_id and should be processed exactly once. Should the initial table be a streaming table or materialized view? Use a streaming table. Kafka is a continuous streaming source, and streaming tables are optimized for processing append only data exactly once. Streaming tables maintain state about which records have been processed, preventing duplicates.
A pipeline computes hourly metrics (counts, averages, percentiles) from raw events. This requires aggregation and statistical calculations. Can this be done with a streaming table or does it need a materialized view? A materialized view is required. Streaming tables are designed for append only transformations. Aggregations like COUNT and PERCENTILE require materializing the results. The materialized view processes the streaming table and computes metrics on demand.
A deduplication pipeline needs to keep only the latest version of each record based on a timestamp. Can this be a streaming table? Yes, streaming tables support stateful operations like deduplication. Use a streaming table with a window function to rank records by timestamp within each group, then keep only rank 1. This is efficient because the streaming table only needs to track new records, not recompute the entire dataset.
---
## Key Takeaways
- Streaming tables process append only data continuously and maintain state between runs.
- Materialized views are recomputed from source data and support arbitrary transformations including aggregations.
- Use streaming tables for continuous ingestion from Kafka, Auto Loader, or other streaming sources.
- Use materialized views for aggregations, joins, and complex transformations that recompute from source.
- A materialized view can process a streaming table as input and refine the data with aggregations.
---
## Gotchas and Tips
⚠️ Streaming tables must read from streaming sources (dlt.read_stream). If you try to read a static table with dlt.read_stream, the pipeline will fail. Use dlt.read() for non streaming sources.
⚠️ Materialized views can become expensive if they recompute complex aggregations over large datasets on every run. Monitor performance and consider filtering data by time window (e.g., CURRENT_TIMESTAMP() - INTERVAL 24 HOURS) to reduce recomputation.
⚠️ Streaming tables output is always append only, even if the input appears to have updates. If you need to handle updates or deletes, use a materialized view with appropriate logic to handle state changes.
---
## Links and Resources
- **[Streaming Tables](https://docs.databricks.com/aws/en/ldp/streaming-tables)** — Defining and managing streaming tables for append-only incremental pipelines.
- **[Materialized Views](https://docs.databricks.com/aws/en/ldp/materialized-views)** — Pre-computed views that automatically refresh when upstream data changes.