It is a file format to store tabular data like ==CSV==. It stores data in row and column hybrid approach.
#### ==Characteristics==
- It is open source, maintained by ==Apache Software Foundation==.
- Contains all the metadata needed to read and write the file. This includes the data types as well.
- Designed to handle ==OLAP== workloads.
- Immutable - Once the file is written it can only be read and can’t be modified.
- It is not human readable.
- It is faster to read compared to write, as OLAP needs more read operations than write operations.
- It provides high performance compression and encoding schemes.
- Supports complex data types like arrays.
#### ==Internal structure==
```
Parquet File
├── Row Group 1
│   ├── Column Chunk (col1) → Pages of data
│   ├── Column Chunk (col2) → Pages of data
│   └── Column Chunk (col3) → Pages of data
├── Row Group 2
│   └── ...
└── Footer (schema + metadata + min/max stats per column)
```
- ==Row Group —== horizontal partition of data (~128MB default).
- ==Column Chunk —== all values for one column within a row group.
- ==Page —== smallest unit of compression and encoding (~1MB).
- ==Footer —== schema and statistics — read first to enable skipping.
#### ==Limitations==
- Not human readable — need tools to inspect.
- Not ideal for frequent small writes — better for bulk writes.
- Schema changes require rewriting files (Delta Lake solves this).
#### ==Event Timeline==
|Year|Event|
|---|---|
|2010|Google publishes **Dremel paper** — the research that inspired Parquet's columnar + nested data design|
|2012|Twitter and Cloudera begin joint development of Parquet using the record-shredding algorithm from Google's Dremel paper|
|Mar 2013|Parquet released as open-source project under Apache Software Foundation|
|Jul 2013|Apache Parquet 1.0 officially released|
|Apr 2015|Parquet becomes a top-level Apache Software Foundation project|
|Aug 2015|Parquet adds support for Hive, Drill, Impala, Pig, Presto, and Spark|
|2016|Parquet 2.0 — major format spec improvements, better encoding support|
|2018|Zstd compression support added — better compression than Snappy/Gzip|
|2019|Delta Lake built on top of Parquet — Parquet becomes foundation of lakehouse architecture|
|2021|Apache Iceberg and Apache Hudi both standardize on Parquet as their underlying file format|
|2022|Parquet encryption (modular encryption) becomes widely supported|
|2023|GeoParquet 1.0 released — geospatial extension of Parquet|
|2024|Parquet format 2.10+ — Variant type support for semi-structured data|
|2025|Parquet format 2.12.0 released — latest stable version|
|2026|Default storage format across all major cloud data platforms — S3, ADLS, GCS, Databricks, Snowflake|
#### ==An idea of Parquet vs CSV for business analytics:==
|Dataset|Size on Amazon S3|Query Run Time|Data Scanned|Cost|
|---|---|---|---|---|
|Data stored as CSV files|1 TB|236 seconds|1.15 TB|$5.75|
|Data stored in Apache Parquet Format|130 GB|6.78 seconds|2.51 GB|$0.01|
|Savings|87% less when using Parquet|34x faster|99% less data scanned|99.7% savings|
# ==References==

> [!info] Documentation  
> Welcome to the documentation for Apache Parquet.  
> https://parquet.apache.org/docs/  

> [!info] What is Parquet? | Databricks  
> What is Parquet?  
> https://www.databricks.com/blog/what-is-parquet  
# ==Must Read==

> [!info]  
>  
> https://www.uber.com/in/en/blog/cost-efficiency-big-data/  
- About compression algorithms and its effect on storage, cost and performance.

> [!info]  
>  
> https://www.uber.com/in/en/blog/one-stone-three-birds-finer-grained-encryption-apache-parquet/  
- About data security using encryption algorithms and challenges and solutions to achieve it.

> [!info] I spent 8 hours learning Parquet. Here’s what I discovered  
> I finally sat down and learned about it.  
> https://vutr.substack.com/p/the-overview-of-parquet-file-format?r=2rj6sg&utm_campaign=post&utm_medium=web&triedRedirect=true  
- Internal workings for parquet format.