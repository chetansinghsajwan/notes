## Topic Overview
Advanced transformations are where the data engineering work happens. You need to master window functions to compute running totals and rank users. You need to understand different join types and when to broadcast small tables. You need to know aggregation patterns and how to use higher order functions for complex transformations.
Window functions let you compute per row values based on a partition and order. ROW_NUMBER gives you a unique row within a partition. LEAD and LAG let you access the previous or next row. RANK handles ties. You can compute running sums, moving averages, and cumulative totals. The Professional exam expects you to write window queries from scratch.
Joins are the foundation of multi table transformations. You need to know when to use INNER, LEFT, RIGHT, FULL OUTER, ANTI, and SEMI joins. Broadcast joins optimize by sending the small table to all executors. Aggregations with CUBE and ROLLUP generate subtotals and grand totals. Higher order functions like transform, filter, and reduce let you work with arrays and maps like functional programming. Master these and you can transform any data.
---
## Key Concepts
Window Functions: OVER, PARTITION BY, ORDER BY
A window function computes values over a window (set of rows). The OVER clause defines the window. PARTITION BY divides rows into groups. ORDER BY sorts within each group. For example, ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY timestamp) gives each user's events a sequence number. LEAD and LAG access adjacent rows. SUM() OVER (PARTITION BY user_id ORDER BY timestamp) computes a running total. The frame (ROWS BETWEEN ...) controls which rows are included in the calculation.
Join Types and When to Use Each
INNER returns rows that match in both tables. LEFT keeps all left rows and matches from the right. RIGHT keeps all right rows and matches from the left. FULL OUTER keeps all rows from both. ANTI returns left rows with no match in the right (useful for filtering). SEMI returns left rows with at least one match in the right (without duplicating if the right has multiple matches). Choose based on whether you want to keep unmatched rows and which table's rows to preserve.
Broadcast Joins for Optimization
If one table is small enough to fit in memory, broadcast it to all executors instead of shuffling both tables. This is much faster. Spark auto broadcasts tables under a threshold (typically 10MB). You can force a broadcast with the broadcast() hint. Use broadcast for dimension tables joining with large fact tables.
CUBE and ROLLUP for Multi level Aggregations
CUBE generates all possible subtotals (all combinations of grouping columns). ROLLUP generates a hierarchy of subtotals (top down grouping). For example, GROUP BY ROLLUP (year, month, day) gives totals by year, by year and month, and by year, month, and day, plus a grand total. Use CUBE when all combinations are meaningful. Use ROLLUP when the grouping has a natural hierarchy.
Higher Order Functions: transform, filter, reduce
transform applies a function to each element of an array: transform(array_col, x -> x * 2). filter keeps elements that match a condition: filter(array_col, x -> x > 100). reduce aggregates an array to a single value: reduce(array_col, 0, (acc, x) -> acc + x). These are functional programming patterns embedded in SQL. Use them instead of explode and collect when possible because they are more efficient.
---
## Code Examples
### Window Functions
```sql
-- ROW_NUMBER for ranking within each user
SELECT 
  user_id,
  event_timestamp,
  ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY event_timestamp) as event_seq
FROM events;
-- Running total of purchases per user
SELECT 
  user_id,
  purchase_date,
  amount,
  SUM(amount) OVER (PARTITION BY user_id ORDER BY purchase_date 
    ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) as cumulative_spent
FROM purchases;
-- LEAD and LAG for comparing rows
SELECT 
  user_id,
  event_date,
  LAG(event_date) OVER (PARTITION BY user_id ORDER BY event_date) as prev_event,
  LEAD(event_date) OVER (PARTITION BY user_id ORDER BY event_date) as next_event,
  DATEDIFF(event_date, LAG(event_date) OVER (PARTITION BY user_id ORDER BY event_date)) as days_since_last
FROM user_activity;
-- RANK vs ROW_NUMBER for handling ties
SELECT 
  department,
  salary,
  ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) as row_num,
  RANK() OVER (PARTITION BY department ORDER BY salary DESC) as salary_rank
FROM employees;
```
### Join Types
```sql
-- INNER JOIN: only matching rows
SELECT o.order_id, c.customer_name, o.amount
FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id;
-- LEFT JOIN: keep all orders, match customers if they exist
SELECT o.order_id, c.customer_name, o.amount
FROM orders o
LEFT JOIN customers c ON o.customer_id = c.customer_id;
-- ANTI JOIN: orders with no matching customer
SELECT o.order_id, o.amount
FROM orders o
LEFT ANTI JOIN customers c ON o.customer_id = c.customer_id;
-- SEMI JOIN: orders that have a matching customer (no duplication)
SELECT DISTINCT o.order_id, o.amount
FROM orders o
LEFT SEMI JOIN customers c ON o.customer_id = c.customer_id;
-- Broadcast join for small dimension
SELECT /*+ BROADCAST(regions) */ 
  s.sale_id, r.region_name, s.amount
FROM sales s
INNER JOIN regions r ON s.region_id = r.region_id;
```
### CUBE and ROLLUP
```sql
-- ROLLUP: hierarchical subtotals (year, month, day levels)
SELECT 
  YEAR(sale_date) as year,
  MONTH(sale_date) as month,
  DAY(sale_date) as day,
  SUM(amount) as total_sales
FROM sales
GROUP BY ROLLUP (YEAR(sale_date), MONTH(sale_date), DAY(sale_date));
-- Returns: subtotals by year, by year+month, by year+month+day, and grand total
-- CUBE: all combinations of grouping columns
SELECT 
  department,
  job_title,
  location,
  COUNT(*) as num_employees,
  AVG(salary) as avg_salary
FROM employees
GROUP BY CUBE (department, job_title, location);
-- Returns: all combinations plus individual subtotals
```
### Higher Order Functions
```sql
-- transform: apply function to each array element
SELECT 
  user_id,
  tags,
  transform(tags, x -> UPPER(x)) as tags_upper
FROM users;
-- filter: keep elements matching condition
SELECT 
  order_id,
  items,
  filter(items, x -> x.price > 100) as expensive_items
FROM orders;
-- reduce: aggregate array to single value
SELECT 
  user_id,
  purchases,
  reduce(purchases, 0, (acc, p) -> acc + p.amount) as total_spent
FROM user_purchases;
-- Complex example: transform prices in array
SELECT 
  order_id,
  items,
  transform(items, item -> named_struct('name', item.name, 'price_after_discount', item.price * 0.9)) as discounted_items
FROM orders;
```
---
## Common Exam Scenarios
Scenario 1: Computing Running Totals by User
You need to calculate each user's cumulative spending over time. Some users have multiple transactions on the same day. How do you ensure the running total is correct even with ties in timestamp?
Use a window function with ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW. This frame includes all rows up to the current row. Add a secondary sort key (transaction ID or row number) to break ties consistently. This ensures reproducible results regardless of processing order.
Scenario 2: Anti Join for Data Quality
You have transactions and a list of blocked customers. You want to find transactions from customers NOT in the blocked list. Which join type should you use?
Use LEFT ANTI JOIN. It returns rows from the left table that have no match in the right table. This is more efficient than LEFT JOIN followed by filtering for NULL values. It does not require any group by or deduplication.
Scenario 3: Broadcast Join for Performance
Your queries join a 10GB fact table (sales) with a 5MB dimension table (stores). The join is slow. What can you do?
Broadcast the stores table. Use the /*+ BROADCAST(stores) */ hint or rely on Spark's auto broadcast if stores is under the threshold. This sends stores to all executors instead of shuffling both tables. It avoids an expensive shuffle and dramatically speeds up the join.
---
## Key Takeaways
- Window functions are essential for analytics. They compute per row metrics without collapsing data like GROUP BY does.
- Always specify the frame (ROWS BETWEEN ...) in window functions to avoid surprises with ties and edge cases.
- Use ANTI JOIN for exclusion filters and SEMI JOIN to find matching rows without duplication.
- Broadcast small tables to avoid shuffles. Check your Spark UI to confirm broadcast joins are happening.
- Higher order functions like transform and filter are more efficient than explode and collect for array operations.
---
## Gotchas and Tips
Window functions can be expensive if the partition is large. Partitioning by user ID works well if data is skewed. Consider repartitioning if one user has millions of rows.
CUBE generates exponentially more rows (2^n combinations). Use it carefully and filter the result to only combinations you care about.
Broadcast only works if the small table fits in memory on all executors. If broadcast fails silently, check the size limit setting (spark.sql.autoBroadcastJoinThreshold).
---
## Links and Resources
- **[SQL Language Reference](https://docs.databricks.com/aws/en/sql/language-manual/)** — Complete SQL reference for window functions, higher-order functions, and complex transformations.
- **[MERGE INTO](https://docs.databricks.com/aws/en/delta/merge)** — Upsert operations combining insert, update, and delete in a single statement.