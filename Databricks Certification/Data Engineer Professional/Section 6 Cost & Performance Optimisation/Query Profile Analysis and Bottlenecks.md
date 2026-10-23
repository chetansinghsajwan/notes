## Topic Overview
Query profiling reveals exactly where your queries spend time and resources. By reading query profiles, you can identify bottlenecks like shuffle operations, spill to disk, and inefficient scans. This knowledge lets you optimize table design, partitioning, and query logic to run faster and cheaper.
The Spark UI shows detailed task execution metrics, including CPU time, I/O time, and memory usage. Photon, Databricks' native query engine, accelerates query execution by orders of magnitude for complex analytical queries. Adaptive query execution automatically adjusts plans based on real runtime statistics.
Understanding bottlenecks is crucial for cost optimization. A query spending 80 percent of time in shuffle typically needs better partitioning or clustering. One spending 80 percent in scan needs data skipping or liquid clustering. Profiling transforms guessing into data driven optimization.
---
## Key Concepts
Spark UI Metrics
The Spark UI displays task execution details: duration, input size, output size, and shuffle read/write. Identifying tasks with long durations or high spill indicates bottlenecks. Memory spill signals that partition sizes are too large or join operations are unbalanced.
Query Profiler
Databricks SQL query profiler shows execution plan with real execution time per node. It identifies expensive operations and helps rewrite queries. Timeline view shows what was happening on the cluster at each moment.
Photon Acceleration
Photon is a native vectorized query engine that speeds up analytical queries. It handles scans, aggregations, and joins faster than Spark. Many queries automatically use Photon when the cluster is configured for it. Identifying non Photon operations can reveal optimization opportunities.
Adaptive Query Execution
Adaptive Query Execution (AQE) changes the execution plan based on runtime statistics. It enables broadcast join optimization, partition coalescing, and skew handling. AQE is transparent and typically improves performance without code changes.
---
## Code Examples
### Accessing Query Profiles
```sql
-- Run a query and profile it
SELECT customer_id, COUNT(*) as order_count
FROM my_catalog.my_schema.orders
GROUP BY customer_id
ORDER BY order_count DESC;
-- Access the query profile via Databricks SQL editor
-- Click "Query Profile" tab to see execution details
-- Or use Python SDK
from databricks.sdk import WorkspaceClient
ws = WorkspaceClient()
statement_execution_id = ws.query("SELECT * FROM my_table LIMIT 1000")
# Use Spark UI to view detailed metrics
```
### Identifying Bottlenecks with Metrics
```sql
-- Query prone to shuffle (indicator of bottleneck)
SELECT o.order_id, c.customer_name, SUM(o.amount) as total
FROM my_catalog.my_schema.orders o
JOIN my_catalog.my_schema.customers c ON o.customer_id = c.customer_id
GROUP BY o.order_id, c.customer_name;
-- Optimized version with clustering
-- Ensure orders and customers are clustered by customer_id
-- This reduces shuffle by collocating matching rows
CREATE TABLE my_catalog.my_schema.orders_optimized (
  order_id BIGINT,
  customer_id BIGINT,
  amount DECIMAL(10, 2)
)
USING DELTA
CLUSTER BY (customer_id);
```
### Enabling Photon
```sql
-- Check if Photon is enabled
SET spark.databricks.photon.enabled = true;
-- Verify in query profile that operations use Photon
-- Photon supported operations: scans, filters, projections, aggregations, joins
-- Some operations may fall back to Spark
-- Check query profile for unsupported operations and optimize if possible
```
---
## Common Exam Scenarios
Scenario 1: Identifying Shuffle Bottleneck
A join query takes 5 minutes with most time spent shuffling. The query profile shows shuffle is moving 50GB across the network. Applying liquid clustering to both tables on the join key reduces shuffle to 500MB. Query now runs in 30 seconds.
Scenario 2: Memory Spill Detection
An aggregation query spills to disk, indicating partitions are uneven. The profile shows some tasks process 10GB while others process 100MB. Repartitioning or using skew hints in the join distributes work more evenly.
Scenario 3: Photon vs Spark Trade Off
Photon accelerates most of the query but a custom UDF forces fallback to Spark. The profile shows the UDF is the bottleneck. Rewriting it in SQL or using a built in function enables Photon for the entire query, improving performance.
---
## Key Takeaways
- Query profilers reveal exactly where time is spent, enabling data driven optimization
- Shuffle, scan, and memory spill are common bottlenecks addressable with clustering and data skipping
- Photon acceleration works transparently but some operations force fallback to Spark
- Adaptive Query Execution optimizes plans automatically without code changes
---
## Gotchas and Tips
Profile Multiple Runs: Query performance varies with data distribution and cluster state. Profile the same query multiple times to get representative metrics.
Photon Limitations: Some UDFs, formats, and operations are not supported by Photon. Check the query profile and Photon documentation for compatibility.
Skew Handling: Highly skewed data leads to uneven task execution. Detect skew in the profile and use Salt joins or Photon skew handling.
---
## Links and Resources
- **[Query Profile](https://docs.databricks.com/aws/en/sql/user/queries/query-profile)** — Analyzing query execution plans to identify bottlenecks and optimize performance.
- **[Spark UI Guide](https://docs.databricks.com/aws/en/optimizations/spark-ui-guide/)** — Diagnosing skew, spill, and shuffle bottlenecks via the Spark UI.