Photon is a high-performance Databricks-native vectorized query engine that runs SQL workloads and DataFrame API calls faster to reduce total cost per workload. It is compatible with Apache Spark APIs, so it works with existing code without any rewrites.
---
**Simple analogy:**
- Standard Spark = a car engine running on JVM (Java).
- Photon = a Formula 1 engine built from scratch in C++ — same roads, same APIs, drastically faster.
---
**Key facts:**
|Property|Detail|
|---|---|
|Written in|C++ (not JVM/Java like Spark)|
|Type|Vectorized query engine|
|Introduced|2021|
|Proprietary?|Yes — Databricks only|
|Code changes needed|None — drop-in replacement|
|Works with|SQL, Python, Scala, R, Java DataFrames|
|Best for|SQL queries, aggregations, joins, ETL on Delta/Parquet|
---
**Why C++ matters — Spark vs Photon:**
|Problem with Spark (JVM)|How Photon fixes it|
|---|---|
|JVM garbage collection pauses|No GC — C++ manages memory directly|
|Row-by-row processing|Vectorized — processes batches of columns at once|
|Cannot use modern CPU SIMD instructions|Directly uses CPU-level optimizations|
|High memory overhead|Lower memory footprint|
---
**Performance numbers:**
|Scenario|Speedup|
|---|---|
|Average across workloads|3x–8x faster vs latest Databricks Runtime|
|TPC-DS 1TB benchmark|Additional 2x speedup on top of DBR|
|Maximum observed|Up to 12x|
|Cost savings (TCO)|Up to 80% TCO savings|
---
**What Photon accelerates:**
SQL and DataFrame operations on Delta and Parquet tables, aggregations and joins, repeated data access from disk cache, and tables with many columns or many small files.
---
**What Photon does NOT accelerate:**
- Pure Python UDFs — these still run on JVM/Python worker
- ML model training (XGBoost, sklearn etc.)
- Pandas UDFs partially benefit
---
**How to enable:**  
Simply select a **Photon-enabled cluster** in Databricks — check the "Photon Acceleration" checkbox when creating a cluster. No code changes needed.
---
**Cost:**  
Photon clusters cost slightly more DBUs (Databricks Units) per hour — but because jobs finish faster, the **net cost is usually lower**.