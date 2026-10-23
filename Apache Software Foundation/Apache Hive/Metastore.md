A **Hive Metastore** is a central repository that stores **metadata** about your data — not the data itself. Think of it as a **library catalogue**: it tells you where books (data) are, what they contain, and how they're organized — but it doesn't store the books.
### What Metadata Does It Store?
|Metadata Type|Example|
|---|---|
|Table names & schemas|`sales_table` has columns: `id`, `amount`, `date`|
|Data location|"This table's files are in S3 bucket X"|
|File format|Parquet, ORC, CSV, etc.|
|Partitioning info|"Data is partitioned by year/month"|
|Database/schema structure|Which tables belong to which database|
### Origin & History
- Created as part of **Apache Hive** — an early Big Data SQL tool built on top of Hadoop (early 2010s)
- Hive needed a way to map SQL table names to actual files sitting in HDFS (Hadoop's file system)
- Databricks later adopted it as the default metadata store — called `hive_metastore` internally
### How It Works (Simply)
```
You run:  SELECT * FROM sales_table
Hive Metastore answers:
→ "sales_table" is at s3://my-bucket/sales/
→ It's in Parquet format
→ It has columns: id (int), amount (float), date (string)
Then Spark reads the actual files from S3
```
### Hive Metastore vs Unity Catalog
|Feature|Hive Metastore (old)|Unity Catalog (new)|
|---|---|---|
|Scope|Per workspace|Across all workspaces|
|Access control|Cluster-level (all users share)|User-level (fine-grained)|
|Data lineage|❌ None|✅ Automatic|
|Multi-cloud|❌ No|✅ Yes|
|Data discovery|❌ Very limited|✅ Rich search & tags|
|Audit logs|❌ No|✅ Yes|
|AI/ML asset governance|❌ No|✅ Yes|
### India Relevance
- Most legacy Databricks deployments in Indian companies (pre-2022) still run on Hive Metastore — **migrating to Unity Catalog** is now a major initiative for Indian IT and BFSI companies.
- Databricks officially deprecated `hive_metastore` as the default — new workspaces default to Unity Catalog.
### Bottom Line
Hive Metastore was a **good-enough solution for its time** but was built for a simpler era — single teams, single clusters, single clouds. Unity Catalog was created specifically because Hive Metastore couldn't scale to modern enterprise needs.