## Topic Overview
The **Spark UI** is the primary diagnostic tool for understanding and optimizing Spark job execution. When a Spark job runs on your Databricks cluster, the UI shows real time metrics about job progress, stage breakdown, task execution, memory usage, and data movement. This information is essential for identifying performance bottlenecks and making targeted optimizations.
You can access the Spark UI from the cluster detail page (in the Compute section) or directly from a job run page in Databricks. The UI is organized into several tabs: Jobs, Stages, Tasks, Storage, Environment, Executors, and SQL/DataFrame. Each tab reveals different aspects of execution. Most exam scenarios involve reading the Spark UI to diagnose performance issues and recommend fixes.
Understanding the Spark UI is critical for optimization. A single slow query might appear fine in isolation, but the UI reveals whether the slowness comes from data skew, excessive shuffling, poor partitioning, or missing broadcast joins. For the exam, you need to recognize these patterns and know the techniques to fix them.
---
## Key Concepts
Navigating the Spark UI
The Jobs tab shows all Spark jobs submitted. Each job is a high level action like a collect() or write() call. Clicking into a job shows its stages. The Stages tab displays stage level details: shuffle read/write bytes, number of tasks, task durations, and data distribution across partitions. Uneven task durations in this tab often indicate data skew. The SQL/DataFrame tab shows the physical execution plan as a Directed Acyclic Graph (DAG). This is where you see whether Spark is using BroadcastHashJoin (efficient) or SortMergeJoin (requires shuffle), how many Exchange (shuffle) nodes are present, and partition counts.
Identifying Data Skew
Data skew occurs when some partitions contain much more data than others, causing some tasks to be significantly slower than the rest. In the Spark UI Stages tab, you see this as a few tasks taking much longer than others while most finish quickly. For example, if 200 tasks complete in 5 seconds but one task takes 45 seconds, that's a classic skew symptom. You can fix skew by salting the join key (adding random values to distribute data more evenly), repartitioning with a better key, or using broadcast joins to avoid the shuffle entirely. Adaptive Query Execution (AQE) has built in skew detection and can automatically adjust.
Shuffle Operations
A shuffle happens when Spark needs to move data across executors. This is expensive because it requires network I/O, disk I/O, and serialization. Common shuffle operations include joins, groupBy, distinct, and repartition. In the Spark UI SQL/DataFrame tab, shuffles appear as Exchange nodes. To minimize shuffle: use broadcast joins for small tables instead of sort merge joins, avoid unnecessary groupBy operations, coalesce partitions instead of repartitioning when possible, and use Adaptive Query Execution to let Spark optimize automatically. The Stages tab shows Shuffle Read and Shuffle Write metrics in bytes. High shuffle amounts mean expensive operations.
Broadcast Joins
A broadcast join is an optimization where a small table is copied to all executors in memory, eliminating the need for a shuffle. Instead of moving data across the network, each executor joins against its local copy of the small table. This is much faster for small table joins. Spark automatically broadcasts tables smaller than `spark.sql.autoBroadcastJoinThreshold` (default 10MB). You can explicitly hint Spark to broadcast a table using the `broadcast()` function. Be cautious with broadcast threshold: broadcasting a 100MB table sends 100MB to every executor, which can consume memory quickly. For the Associate exam, knowing when to use broadcast joins and how to enable them is important.
Partition Tuning
The number of partitions affects parallelism and shuffle size. Too few partitions means fewer tasks and lower parallelism. Too many partitions means more task overhead and smaller data chunks. `spark.sql.shuffle.partitions` controls the number of partitions after a shuffle (default 200). Use `coalesce()` to reduce partitions without reshuffling (data from multiple partitions moves to fewer executors). Use `repartition()` to increase partitions or redistribute by a column (both trigger a shuffle). For exam questions, remember coalesce is cheaper than repartition.
Adaptive Query Execution (AQE)
Adaptive Query Execution is enabled by default in Databricks. AQE automatically optimizes a query plan during execution based on runtime statistics. It does three main things: (1) dynamically adjusts shuffle partition count based on actual data size, (2) converts sort merge joins to broadcast joins if one side turns out to be small, and (3) detects and handles data skew in join keys by splitting large partitions. AQE reduces the need for manual tuning and catches optimization opportunities that the optimizer couldn't predict at plan time. For the exam, understand that AQE is active by default and you don't need to enable it.
Caching Strategies
Caching a DataFrame stores its data in memory (or disk) so subsequent actions don't recompute it. Use `df.cache()` (or `df.persist()` with a storage level) for DataFrames accessed multiple times. The Spark UI Storage tab shows cached DataFrames, their size, and how much memory is used. Cache large DataFrames that are reused often, but avoid caching one off transformations. Also remember that cache doesn't persist across job runs; it's per session.
Photon Acceleration
Photon is a high performance vectorized query execution engine available on Databricks. It accelerates certain SQL and DataFrame operations by using native code instead of Java. Photon is enabled on certain cluster types and handles filtering, aggregations, and joins more efficiently. You don't need to change code to use Photon; it kicks in automatically for supported operations. For the exam, just know that Photon accelerates query execution through vectorization.
---
## Code Examples
**Broadcast Join Hint (SQL and PySpark)**
```sql
-- SQL: use BROADCAST hint
SELECT *
FROM large_table l
JOIN /*+ BROADCAST(s) */ small_table s
  ON l.id = s.id
```
```python
# PySpark: use broadcast() function
from pyspark.sql.functions import broadcast
large_df = spark.read.table("large_table")
small_df = spark.read.table("small_table")
result = large_df.join(broadcast(small_df), "id")
```
**Repartitioning and Coalescing (PySpark)**
```python
# Increase partitions and redistribute by a column (triggers shuffle)
df_repartitioned = df.repartition(100, "user_id")
# Reduce partitions without shuffle (for final write)
df_coalesced = df.coalesce(4)
# You would use coalesce before writing to reduce output file count
df_coalesced.write.mode("overwrite").parquet("/output/path")
```
**Setting Shuffle Partitions (PySpark)**
```python
# Set default shuffle partitions for the session
spark.conf.set("spark.sql.shuffle.partitions", "400")
# Now all shuffle operations use 400 partitions
df_grouped = df.groupBy("category").count()
# Reset to default
spark.conf.set("spark.sql.shuffle.partitions", "200")
```
**Caching a DataFrame (PySpark)**
```python
from pyspark import StorageLevel
# Cache in memory
df.cache()
# Or explicitly specify storage level
df.persist(StorageLevel.MEMORY_ONLY)
# Cache with disk spillover (if memory is full, spill to disk)
df.persist(StorageLevel.MEMORY_AND_DISK)
# Trigger materialization and check Storage tab in Spark UI
df.count()
# Unpersist when done
df.unpersist()
```
---
## Common Exam Scenarios
**Scenario 1: High Task Duration Variance**
You check the Spark UI Stages tab for a join operation. You see 200 tasks, and most complete in 8 seconds, but one task takes 2 minutes. The Shuffle Read metric shows most tasks read 5MB but the slow task read 500MB. This is a classic data skew problem. The join key has skewed distribution—one key value appears in many more rows. To fix this, you could salt the join key (add a random suffix to split the skewed key across multiple tasks) or use Adaptive Query Execution to detect and handle the skew automatically. In this case, since AQE is enabled by default, it should already be trying to handle it, but explicit salting gives you more control.
**Scenario 2: Excessive Shuffle Operations**
Your Spark UI SQL tab shows a chain of Exchange (shuffle) nodes in the DAG. Your query joins a large table with a small lookup table. The Shuffle Write metric is 2GB. You could optimize by using a broadcast join on the small table to eliminate that shuffle entirely. Check the size of the small table; if it's under 10MB (or whatever your threshold is set to), you could explicitly use `broadcast(small_df)`. This avoids the shuffle and often reduces execution time by an order of magnitude.
**Scenario 3: Poor Partition Count Choice**
You have a 100GB DataFrame that you need to write to Parquet. The default `spark.sql.shuffle.partitions` is 200, so Spark creates 200 output files. Each file is ~500MB. You want fewer, larger files for downstream consumption. Use `coalesce(50)` to merge 200 partitions into 50 without reshuffling. This creates 50 output files (~2GB each) and is faster than repartition because it doesn't trigger a shuffle.
---
## Key Takeaways
- The Spark UI is your window into job execution. Use it to spot slow stages, data skew, excessive shuffling, and memory issues.
- Data skew (uneven task durations) is a common performance issue. Fix it with salting, better partitioning, or broadcast joins.
- Shuffles are expensive. Use broadcast joins for small tables, avoid unnecessary groupBy operations, and let Adaptive Query Execution optimize automatically.
- Use `coalesce()` to reduce partitions without shuffle, and `repartition()` to increase or redistribute partitions (both have trade offs).
- Cache frequently accessed DataFrames to avoid recomputation. Check the Storage tab in Spark UI to verify cache usage.
---
## Gotchas and Tips
Broadcast threshold is per executor. If you set `spark.sql.autoBroadcastJoinThreshold` to 100MB and broadcast a 100MB table to 10 executors, you use 1GB of memory total. Broadcasting massive tables can exhaust memory quickly.
Coalesce does not trigger a shuffle but may cause uneven data distribution and slower writes. Repartition triggers a shuffle but redistributes data evenly. Use coalesce only when you don't care about data distribution (e.g., final write where file count is the main concern).
Cache is session specific and not persistent across cluster restarts or job runs. If you cache a DataFrame and then submit a new job, that cache is gone. Use Delta tables or external storage for persistent caching.
---
## Links and Resources
- **[Diagnose cost and performance issues using the Spark UI](https://docs.databricks.com/aws/en/optimizations/spark-ui-guide/)** — Complete guide to reading and interpreting the Spark UI for optimization.
- **[Debugging with the Spark UI](https://docs.databricks.com/aws/en/compute/troubleshooting/debugging-spark-ui)** — Step by step debugging using the Spark UI for skew, spill, and shuffle issues.