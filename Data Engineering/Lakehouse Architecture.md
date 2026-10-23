A **lakehouse** combines the best of **data lakes** (raw storage) and **data warehouses** (structured analytics) into a single platform.
It is an architecture which uses Data Lakes as the storage layer and compute separate.
- It was introduced in a research paper (==Lakehouse: A New Generation of Open Platforms that Unify Data Warehousing and Advanced Analytics==) by ==Databricks researchers== in ==2021==.
- As of 2026 May, no single entity controls the Lakehouse architecture.
### ==Core Idea==
- Store raw data cheaply (like a lake)
- Query it with warehouse-like performance & structure
- No need to move data between systems
### ==Key Components==
|Component|Role|
|---|---|
|**Open File Formats**|Parquet, ORC, Avro for storage|
|**Metadata Layer**|Delta Lake, Apache Iceberg, Apache Hudi|
|**Query Engine**|Spark, Trino, Presto, DuckDB|
|**Storage**|S3, ADLS, GCS, HDFS|
|**Governance Layer**|Unity Catalog, Apache Atlas|
### Lakehouse vs Others
|Feature|Data Lake|Data Warehouse|Lakehouse|
|---|---|---|---|
|Cost|Low|High|Low-Medium|
|Structure|None|Rigid|Flexible|
|ACID Transactions|❌|✅|✅|
|ML Support|✅|❌|✅|
|Real-time|Partial|Partial|✅|
### Popular Platforms
|Platform|Provider|
|---|---|
|Databricks|Databricks|
|BigLake|Google Cloud|
|Microsoft Fabric|Azure|
|AWS Glue + S3|Amazon|
|Apache Iceberg + Spark|Open Source|