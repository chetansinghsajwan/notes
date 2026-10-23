## Topic Overview
The Spark UI and Query Profiler are your windows into how Databricks executes your code. The Spark UI shows the stage DAG (directed acyclic graph), task execution timelines, and data movement across the cluster. The SQL/DataFrame query profiler gives you a detailed execution plan showing where time and resources are spent. Together, they help you identify bottlenecks like shuffles, data skew, and inefficient operations.
Learning to read execution plans and Spark UI visualizations is critical for performance optimization. You'll spot patterns like unnecessary shuffles, missing indexes, joins on mismatched data types, or stages that process tiny partitions. The query profiler lets you understand if time is spent scanning data, executing operations, or moving data between nodes.
## Key Concepts
**Spark UI Stages, Tasks, and DAG**
A stage is a group of tasks that can run in parallel. The DAG (directed acyclic graph) shows how stages depend on each other. A task is the atomic unit of work executed by a single executor on a partition. The Spark UI displays the stage DAG visually, showing which stages run sequentially and which depend on shuffles. Wide transformations (like joins and group bys) create shuffle boundaries and multiple stages.
**Execution Plan and EXPLAIN**
The execution plan is a tree of operations that Databricks will execute to compute your result. EXPLAIN EXTENDED shows both the logical plan (what you asked for) and physical plan (how Databricks will do it). The physical plan includes details about joins (broadcast vs sort merge), filters (predicate pushdown), and data flow. Read from top to bottom to understand the order of operations.
**Shuffles, Spills, and Skew**
A shuffle is when data moves between executors, usually because of a join or group by. Shuffles are expensive. A spill occurs when intermediate data doesn't fit in memory and is written to disk. Data skew means some tasks process much more data than others, creating a bottleneck. The Spark UI highlights these in red. Looking at task execution timelines helps you spot skewed partitions.
**SQL Query Profiler**
The SQL query profiler (available in Databricks SQL editor) shows timing at each step of the execution plan. It breaks down where time is spent: scanning, filtering, joining, writing, etc. You can click on stages to see metrics like rows processed, bytes read, and execution time. This is more granular than the Spark UI.
---
## Code Examples
### Reading Execution Plans with EXPLAIN
```sql
-- Simple EXPLAIN shows the physical plan
EXPLAIN SELECT 
  o.customer_id,
  COUNT(*) as order_count,
  SUM(o.amount) as total_spent
FROM orders o
JOIN customers c ON o.customer_id = c.id
WHERE c.region = 'US'
GROUP BY o.customer_id;
-- EXPLAIN EXTENDED shows both logical and physical plans
EXPLAIN EXTENDED SELECT * FROM gold_sales WHERE year = 2024;
-- Key terms to look for in execution plans:
-- BroadcastHashJoin = efficient for small tables
-- SortMergeJoin = used for large joins, requires shuffle
-- Scan = reading from storage
-- Filter = WHERE clause applied
-- Shuffle = data moving between nodes
-- Exchange = shuffle boundary
```
### DataFrame API with Query Inspection
```python
from pyspark.sql.functions import col, sum as spark_sum
# Read data
orders_df = spark.read.table("orders")
customers_df = spark.read.table("customers")
# Complex query
result = (orders_df
  .join(customers_df, orders_df.customer_id == customers_df.id, "inner")
  .filter(col("region") == "US")
  .groupBy("customer_id")
  .agg(spark_sum("amount").alias("total_spent")))
# Inspect the execution plan
result.explain()
# Inspect the plan in detail
result.explain(mode="extended")
# Get the SQL representation
result.createOrReplaceTempView("temp_result")
spark.sql("EXPLAIN EXTENDED SELECT * FROM temp_result").show(truncate=False)
```
### Identifying Performance Issues in Execution Plans
```sql
-- Query with potential performance issues
EXPLAIN EXTENDED
SELECT 
  o.order_id,
  c.name,
  p.price
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
JOIN products p ON o.product_id = p.product_id
WHERE o.year = 2024;
-- Things to look for in the plan:
-- 1. SortMergeJoin is expensive - check if broadcasting a small table would help
-- 2. Multiple shuffles in sequence - can you reduce the number of joins?
-- 3. Filter applied after join - push filters down before the join
-- 4. Exchange (shuffle) happening early - apply filters first to reduce data
-- Better approach: push filters early
EXPLAIN EXTENDED
SELECT 
  o.order_id,
  c.name,
  p.price
FROM orders o
WHERE o.year = 2024  -- Filter early!
JOIN customers c ON o.customer_id = c.customer_id
JOIN products p ON o.product_id = p.product_id;
```
---
## Common Exam Scenarios
Scenario: A data analyst runs a query that takes much longer than expected. The Spark UI shows that one stage has 100 tasks, but one task is taking 10 times longer than the others. What does this indicate?
This is data skew. One partition has much more data than the others, creating a bottleneck. To fix it, you might repartition the data on a different column, use salt keys to distribute skewed values, or rewrite the query to avoid the problematic join. The Spark UI's task timeline visualization is the key to spotting this.
Scenario: Your EXPLAIN output shows a SortMergeJoin between a large table and a small lookup table. How would you optimize this?
Use a broadcast join instead. Check the size of the lookup table. If it's small enough (typically under 2GB), Spark should automatically broadcast it, but you can force it with the broadcast() hint: `df1.join(broadcast(df2), ...). This eliminates the shuffle and makes the join much faster.
Scenario: Your execution plan shows multiple Exchange (shuffle) operations in sequence. What does this mean and how do you minimize it?
Multiple exchanges mean you have multiple wide transformations (joins, group bys) stacked together, causing data to shuffle multiple times. Each shuffle is expensive. Minimize this by combining operations when possible. For example, if you join table A with B, then with C, see if you can join B and C first and then join with A in one operation. Or use window functions instead of multiple group bys if applicable.
---
## Key Takeaways
- The Spark UI shows stages, tasks, and the DAG. The query profiler breaks down timing by operation. Use both to identify bottlenecks.
- EXPLAIN shows the execution plan. Look for SortMergeJoin, unnecessary shuffles, and opportunities to push filters early.
- Data skew (uneven partition sizes) shows up as tasks with very different execution times. Use repartitioning or salt keys to fix it.
- Use broadcast() to hint small table joins. Avoid multiple shuffles in sequence by combining operations.
---
## Gotchas and Tips
The Spark UI is only available while the job is running or for a short time after completion. Once the cluster shuts down or a long time passes, the UI is archived. Access it quickly during execution or enable event log persistence for later analysis.
EXPLAIN shows what Databricks plans to do, but actual execution might differ based on runtime statistics. Always verify with the Spark UI to see what actually happened.
Broadcast joins work well for small tables. If the table is too large, Databricks automatically falls back to SortMergeJoin. Check your table sizes before relying on broadcast.
---
## Links and Resources
- **[Query Profile](https://docs.databricks.com/aws/en/sql/user/queries/query-profile)** — Analyzing query execution plans and identifying performance bottlenecks.
- **[Spark UI Guide](https://docs.databricks.com/aws/en/optimizations/spark-ui-guide/)** — Understanding Spark UI stages, tasks, and execution metrics.