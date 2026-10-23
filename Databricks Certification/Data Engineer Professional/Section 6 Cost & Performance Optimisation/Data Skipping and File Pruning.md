## Topic Overview
Data skipping is one of Delta Lake's most powerful performance features. When you query a table with a WHERE clause, Delta uses min and max statistics stored in the transaction log to skip files that cannot possibly contain matching rows. This means your query engine doesn't have to scan every file, dramatically reducing I/O and query time.
File pruning takes this further by using column statistics to eliminate files before the actual scan starts. For example, if you query WHERE date = '2026-01-15' and a file's statistics show all dates in that file are from 2025, that file is skipped entirely. Bloom filters add another layer, enabling skipping for specific values within files.
Understanding how statistics are collected and used is essential for optimizing queries. Different data types and storage organizations affect which skipping techniques apply. Z-order and liquid clustering both improve skipping by co locating related data.
---
## Key Concepts
Min Max Statistics
Delta stores minimum and maximum values for columns in each file. When you filter on a column, Delta checks if a file's min/max range could contain matching rows. If your filter is WHERE id > 1000 and a file has max id of 500, that file is skipped entirely.
Column Statistics Collection
Statistics are automatically collected when data is written. You can manually collect or update statistics with the ANALYZE command. More comprehensive statistics enable better skipping decisions but require extra processing during writes.
Bloom Filters
Bloom filters store a compact probabilistic representation of values in a file. They enable skipping even for non comparable types and provide finer grained skipping than min/max statistics alone. Bloom filters have a small memory footprint but are more CPU intensive to evaluate.
Predicate Pushdown
Predicate pushdown applies filters before scanning actual data. Combined with data skipping, predicates eliminate files at the planning stage, further reducing the data engine needs to process. This is especially powerful in distributed query execution.
---
## Code Examples
### Viewing Table Statistics
```sql
-- View table statistics
DESC DETAIL my_catalog.my_schema.events;
-- Check column statistics
DESC FORMATTED my_catalog.my_schema.events;
-- Analyze and collect statistics
ANALYZE TABLE my_catalog.my_schema.events COMPUTE STATISTICS;
ANALYZE TABLE my_catalog.my_schema.events COMPUTE STATISTICS FOR COLUMNS user_id, event_type;
```
### Enabling Bloom Filters
```sql
-- Enable Bloom filters for specific columns
ALTER TABLE my_catalog.my_schema.users
SET TBLPROPERTIES ('delta.dataSkippingNumIndexedCols' = '5');
-- Create table with data skipping optimization
CREATE TABLE my_catalog.my_schema.orders (
  order_id BIGINT,
  customer_id BIGINT,
  order_date DATE,
  amount DECIMAL(10, 2)
)
USING DELTA
TBLPROPERTIES ('delta.enableDeletionVectors' = 'true',
               'delta.dataSkippingNumIndexedCols' = '3');
```
### Query Optimization with Data Skipping
```sql
-- Queries that benefit from data skipping
-- Single column filter
SELECT * FROM my_catalog.my_schema.events
WHERE event_date = '2026-02-15';
-- Range filter
SELECT * FROM my_catalog.my_schema.transactions
WHERE created_at BETWEEN '2026-01-01' AND '2026-02-01';
-- Compound filter
SELECT * FROM my_catalog.my_schema.users
WHERE country = 'US' AND signup_date > '2025-01-01';
```
---
## Common Exam Scenarios
Scenario 1: Query Performance on Partitioned Data
A table stores 3 years of event data partitioned by date. A query filters on a specific date. Without data skipping, the engine scans files from all dates. With skipping enabled, min max statistics show which files contain the target date, and other files are skipped entirely. This can reduce I/O by 90 percent or more.
Scenario 2: Bloom Filters for Non Partitioned Data
A table has user IDs scattered across many files with no particular organization. Bloom filters on user_id column enable skipping files that definitely do not contain a specific user. This provides skipping benefits even when data is not partitioned or clustered.
Scenario 3: Compound Filters with Predicate Pushdown
A query has WHERE region = 'EU' AND revenue > 100000. Predicate pushdown evaluates both conditions early, and data skipping eliminates files that do not overlap the specified ranges. Statistics on both columns work together to minimize data scanned.
---
## Key Takeaways
- Min max statistics enable Delta to skip files based on column value ranges
- Bloom filters provide finer grained skipping than statistics alone for specific value searches
- Statistics are automatically collected when data is written and can be manually updated with ANALYZE
- Data skipping works best with filters on columns that have good selectivity and are frequently queried
---
## Gotchas and Tips
Statistics Staleness: Column statistics can become stale if data is frequently updated without re analyzing. Stale statistics lead to ineffective skipping. Automate ANALYZE in your pipelines for high velocity tables.
Bloom Filter Overhead: Bloom filters add small overhead during writes. For tables where data skipping is not beneficial (random access patterns), avoid enabling too many Bloom filters.
Column Type Matters: Data skipping works differently for different types. Numeric columns benefit most from min max statistics. String columns benefit more from Bloom filters.
---
## Links and Resources
- **[Data Skipping](https://docs.databricks.com/aws/en/delta/data-skipping)** — Leveraging column-level statistics to skip irrelevant files during queries.
- **[Dynamic File Pruning](https://docs.databricks.com/aws/en/optimizations/dynamic-file-pruning)** — Runtime optimization that prunes files based on join predicates.