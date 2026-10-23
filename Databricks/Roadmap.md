Great question. Here's the complete Databricks learning roadmap, ordered by what to learn first:
---
**Prerequisites (before Databricks):**
|Topic|Why needed|
|---|---|
|SQL (intermediate+)|Everything in Databricks runs on SQL or Spark SQL|
|Python (intermediate+)|PySpark is Python-based|
|Basic cloud (AWS/Azure/GCP)|Databricks runs on cloud|
|Git|Version control for notebooks and pipelines|
---
**Module 1 — Databricks Fundamentals:**
|Topic|Key concepts|
|---|---|
|Workspace & UI|Notebooks, clusters, repos, jobs|
|Cluster types|All-purpose vs job clusters, autoscaling|
|Notebook basics|Python, SQL, Scala, R cells|
|DBFS|Databricks File System — how storage works|
|Magic commands|`%sql`, `%python`, `%fs`, `%md`|
---
**Module 2 — Apache Spark (core engine):**
|Topic|Key concepts|
|---|---|
|Spark architecture|Driver, executors, partitions|
|RDDs|Low-level — understand but don't use daily|
|DataFrames|Primary abstraction you'll use|
|Spark SQL|SQL on DataFrames|
|Transformations vs Actions|Lazy evaluation concept|
|Joins|Broadcast join, shuffle join, skew handling|
|Partitioning|`repartition` vs `coalesce`|
|Caching|`cache()`, `persist()`|
|UDFs|User-defined functions in Python/SQL|
---
**Module 3 — Delta Lake (most important for your work):**
|Topic|Key concepts|
|---|---|
|Delta table basics|Creating, reading, writing|
|ACID transactions|How Delta guarantees consistency|
|Schema enforcement|Prevent bad data at write time|
|Schema evolution|Add new columns safely|
|Time travel|Query older versions of data|
|MERGE (upsert)|Core of SCD Type 2 — you already know this|
|OPTIMIZE & ZORDER|Performance tuning|
|VACUUM|Clean up old files|
|Change Data Feed|Track row-level changes|
|Table properties|`delta.logRetentionDuration` etc.|
---
**Module 4 — Medallion Architecture:**
|Topic|Key concepts|
|---|---|
|Bronze layer|Raw ingestion, no transformation|
|Silver layer|Cleaned, joined, deduplicated|
|Gold layer|Aggregated, business-ready|
|SCD Type 1 / Type 2|Slowly changing dimensions — you already know|
|Data modeling|Star schema, snowflake schema on Delta|
---
**Module 5 — Databricks SQL:**
|Topic|Key concepts|
|---|---|
|SQL Warehouses|Serverless vs classic|
|Dashboards|Built-in BI layer|
|Alerts|Threshold-based notifications|
|Query history & profiling|Performance debugging|
|Array/map functions|Complex type operations — you already know|
---
**Module 6 — Delta Live Tables (DLT):**
|Topic|Key concepts|
|---|---|
|DLT pipelines|Declarative pipeline definition|
|`@dlt.table` decorator|Define tables as functions|
|Expectations|Data quality rules built-in|
|Streaming vs batch|Same code handles both|
|Pipeline modes|Development vs production|
---
**Module 7 — Workflows & Orchestration:**
|Topic|Key concepts|
|---|---|
|Databricks Workflows|Job scheduling and orchestration|
|Multi-task jobs|DAG of tasks with dependencies|
|Job clusters|Ephemeral clusters for cost saving|
|Triggers|Time-based, file-arrival, API|
|Alerts & notifications|Email/Slack on failure|
|Integration with Airflow|For external orchestration|
---
**Module 8 — Unity Catalog & Governance:**
|Topic|Key concepts|
|---|---|
|Metastore setup|One per region|
|3-level namespace|`catalog.schema.table`|
|Access control|Row/column level security|
|Data lineage|Auto-captured lineage|
|Tags & classification|PII tagging|
|Audit logs|Who accessed what|
---
**Module 9 — Performance Tuning:**
|Topic|Key concepts|
|---|---|
|Query plan reading|`EXPLAIN` command|
|Shuffle optimization|Reduce expensive shuffles|
|Skew handling|Salting, AQE|
|Photon engine|Vectorized C++ query engine in Databricks|
|Liquid clustering|Replacement for static ZORDER|
|File size optimization|Small file problem|
|Caching strategies|Disk cache vs memory cache|
---
**Module 10 — Streaming:**
|Topic|Key concepts|
|---|---|
|Structured Streaming|Spark's streaming API|
|Trigger modes|Once, continuous, fixed interval|
|Watermarking|Handle late-arriving data|
|Kafka integration|Most common source|
|Auto Loader|Incrementally ingest from cloud storage|
|Checkpointing|Fault tolerance for streams|
---
**Module 11 — ML & AI (optional for data engineers):**
|Topic|Key concepts|
|---|---|
|MLflow|Experiment tracking, model registry|
|Feature Store|Reusable ML features|
|AutoML|No-code model training|
|Model serving|REST endpoint for predictions|
|Mosaic AI|Databricks' LLM/GenAI platform|
---
**Certifications (in order to attempt):**
|Certification|Level|Exam fee (INR approx.)|
|---|---|---|
|Databricks Certified Associate Developer for Apache Spark|Beginner|₹17,000|
|Databricks Certified Data Engineer Associate|Beginner|₹17,000|
|Databricks Certified Data Engineer Professional|Advanced|₹25,000|
|Databricks Certified ML Associate|Beginner|₹17,000|
---
**Recommended learning order for you** (given your current experience):
```
Delta Lake deep dive (Module 3)
    → Performance tuning (Module 9)
        → DLT (Module 6)
            → Unity Catalog (Module 8)
                → Streaming (Module 10)
                    → Certifications
```
You can skip Modules 1–2 basics and most of Module 5 since you're already working hands-on.