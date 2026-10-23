It is a framework to add functionality on top [[Data Lake]] to integrate them into the [[Lakehouse Architecture]].
### ==Characteristics==
- Open source - developed by Databricks and then handed over to Linux Foundation as open source project.
- Website - [https://delta.io](https://delta.io/)
- GitHub repo - https://github.com/delta-io/delta
### ==Features==
Delta Lake is build on top of parquet format, so it leverages all the features already provided by the [[Parquet Format]].
|Feature|What it does|Provided by|
|---|---|---|
|Columnar Storage|Data stored column-by-column for faster analytical reads|Parquet|
|Compression|Automatically compresses data on disk to reduce storage size|Parquet|
|Schema Enforcement (read-time)|Validates data matches expected schema when reading|Parquet|
|Predicate Pushdown|Skips column chunks if filter can be resolved from metadata|Parquet|
|Column Pruning|Reads only columns needed for a query, ignores the rest|Parquet|
|Nested Data Types|Supports arrays, maps, and structs within a column|Parquet|
|Row Group Min/Max Statistics|Stores min/max per column chunk for fast filtering|Parquet|
|ACID Transactions|Serializable isolation — no partial or inconsistent writes|Delta Lake|
|Transaction Logs|Audit logs of every operation; source of truth for table state|Delta Lake|
|Time Travel|Query or restore table at any past version or timestamp|Delta Lake|
|Schema Enforcement (write-time)|Rejects writes that don't match the table schema|Delta Lake|
|Schema Evolution|Safely add/widen columns over time with full audit trail|Delta Lake|
|Scalable Metadata Handling|Uses Spark to handle metadata for petabyte-scale tables with billions of files|Delta Lake|
|Streaming + Batch Unification|Same table works as batch table, streaming source, and streaming sink|Delta Lake|
|DML — UPDATE / DELETE / MERGE|Row-level SQL operations directly on Delta tables|Delta Lake|
|Change Data Feed (CDC)|Tracks row-level inserts, updates, deletes for downstream consumers|Delta Lake|
|Delta Sharing|Open protocol to share Delta tables across orgs without copying data|Delta Lake|
|Z-Ordering / Data Skipping|Co-locates related data so queries skip irrelevant files|Delta Lake|
|OPTIMIZE|Compacts many small files into larger ones for faster reads|Delta Lake|
|VACUUM|Deletes old Parquet files no longer needed by any version|Delta Lake|
|UniForm|Lets Iceberg or Hudi clients read Delta tables without conversion|Delta Lake|
|Coordinated Commits|Safe concurrent writes from multiple engines without conflicts|Delta Lake|
|Variant Data Type|Flexible high-performance storage for semi-structured JSON data|Delta Lake|
|Identity Columns|Auto-incrementing unique IDs for rows like a database sequence|Delta Lake|
|Collations|Language- and case-aware sorting and string comparisons|Delta Lake|
|Type Widening|Columns can evolve to wider data types safely with full audit trail|Delta Lake|
|Row Tracking|Tracks individual rows across inserts, updates, deletes; enables efficient CDC|Delta Lake|
|Clustered Tables|Defines clustering columns for faster file skipping and query performance|Delta Lake|
|Catalog-Managed Tables|Integrates with external catalogs; foundation for foreign keys and observability|Delta Lake|
|Delta Connect|Full Apache Spark Connect support for remote/thin-client connectivity|Delta Lake|
### ==Internal Structure==
```
Delta Table
├── _delta_log/                    # transaction log
│   ├── 00000000000000000000.json  # commit 0 — table created
│   ├── 00000000000000000001.json  # commit 1 — rows inserted
│   ├── 00000000000000000002.json  # commit 2 — rows updated
├── part-00001.parquet             # actual data files
├── part-00002.parquet
```
- Data is stored as **Parquet files**.
- Every change is recorded in the `**_delta_log**` — this is what makes Delta Lake special.
- Queries read the log to find the latest valid data files.
### ==Event Timeline==
|Date|Milestone & Significance|
|---|---|
|2017|Databricks Delta announced at Spark Summit — ACID on data lakes introduced; internal-only|
|Apr 2019|Delta Lake 0.1 open sourced (Apache 2.0) — ACID transactions, time travel, schema enforcement made public|
|Oct 2019|Joined Linux Foundation — neutral governance; no longer solely Databricks-led|
|May 2021|Delta Lake 1.0 stable release — first production-stable version; announced by Michael Armbrust at Data+AI Summit|
|Jun 2022|Delta Lake 2.0 fully open sourced — all Databricks-exclusive features (Delta Sharing, CDF, DML) now OSS|
|Oct 2023|Delta Lake 3.0 (Spark 3.5) — UniForm preview; read Delta tables as Iceberg or Hudi|
|May 2024|Moved to LF AI & Data Foundation — fully community-governed; no single company in control|
|Jun 2025|Delta Lake 4.0 — biggest release ever (Spark 4.0); Delta Connect, Variant type, Coordinated Commits, Identity Columns|
|Apr 2026|Delta Lake 4.2 (latest) — catalog-managed tables, incremental improvements|
### ==Restore back to old version==
```jsx
-- By version
RESTORE TABLE catalog.schema.my_table TO VERSION AS OF 5
-- By timestamp
RESTORE TABLE catalog.schema.my_table TO TIMESTAMP AS OF '2025-01-01'
```
# ==References==

> [!info] Welcome to the Delta Lake documentation  
> Learn how to use Delta Lake  
> https://docs.delta.io/  

> [!info] What is Delta Lake in Databricks? | Databricks on AWS  
> Learn about the Delta Lake storage protocol used to power the Databricks lakehouse.  
> https://docs.databricks.com/aws/en/delta/  

> [!info] Delta Lake vs Data Lake - What's the Difference? | Delta Lake  
> This article defines Delta Lake and Data Lake and explains the differences you should know when deciding how to store your data.  
> https://delta.io/blog/delta-lake-vs-data-lake/  

> [!info] Introduction to Data Lakes | Databricks  
> Discover how Databricks' data lakes provide a unified platform for managing big data at scale, enabling advanced analytics, AI, and machine learning.  
> https://www.databricks.com/discover/data-lakes