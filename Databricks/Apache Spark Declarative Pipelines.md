Spark Declarative Pipelines (SDP) is a declarative framework for building reliable, maintainable, and testable data pipelines on Spark. It simplifies ETL development by allowing you to focus on the transformations you want to apply to your data, rather than the mechanics of pipeline execution. It is designed for both batch and streaming data processing.
**The key idea:**
- You define **what** datasets should exist and what their contents should be.
- SDP handles **how** — orchestration, execution order, retries, incremental processing, and error handling automatically.
**Simple analogy:**
- Writing PySpark manually = giving turn-by-turn directions.
- SDP = just saying the destination, GPS figures out the route.
**Core building blocks:**
|Concept|What it does|
|---|---|
|Flow|Single processing step — reads, transforms, writes|
|Streaming Table|Continuously ingests from a stream source|
|Materialized View|Stores batch query results, auto-refreshed on source change|
|Auto CDC|Handles change data capture with SCD Type 1/2 (Lakeflow only)|
**Origin in one line:**  
SDP is the open-source version of Databricks' **Delta Live Tables (DLT)**, contributed to Apache Spark in June 2025.