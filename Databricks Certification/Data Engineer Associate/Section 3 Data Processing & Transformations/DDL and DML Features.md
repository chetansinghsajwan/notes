## Topic Overview
DDL and DML are the core languages for managing data in Databricks. DDL (Data Definition Language) creates and modifies table structures, while DML (Data Manipulation Language) handles data operations like inserts, updates, and deletes. What makes Databricks special is that Delta Lake extends both with powerful features like time travel, atomic transactions, and upsert operations that aren't available with traditional data warehouses.
For the Associate exam, you need to understand how to create and modify tables, query data at different versions, optimize table performance, and use MERGE for upsert patterns. The exam heavily tests your knowledge of Delta specific capabilities like time travel and OPTIMIZE/VACUUM commands.
These commands are used every day in production data pipelines. Understanding them well means you can build efficient, maintainable data solutions and debug issues when they arise.
---
## Key Concepts
MERGE INTO: The Upsert Pattern
MERGE INTO combines INSERT, UPDATE, and DELETE into a single atomic operation. You specify a condition to match rows in the target table. When rows match, you can UPDATE them. When they don't match, you can INSERT new rows. You can even DELETE rows that match certain conditions. This is essential for loading slowly changing dimensions and handling incremental updates from source systems.
CREATE OR REPLACE TABLE (CRAS)
CRAS atomically replaces table content while preserving the Delta log and table history. Unlike DROP and recreate, CRAS is atomic (all or nothing) and maintains the table's version history. This is useful for full refresh patterns and batch rebuilds. The table metadata (location, schema) stays the same.
Time Travel: Querying Historical Data
Delta Lake stores a detailed log of every transaction. You can query data as it was at any point in time using VERSION AS OF (by version number) or TIMESTAMP AS OF (by date/time). You can also restore a table to a previous version. This is critical for debugging data issues and recovering from mistakes.
OPTIMIZE: Compacting Files
Each write to a Delta table creates one or more Parquet files. Over time, lots of small files accumulate, slowing down queries. OPTIMIZE rewrites the table into larger files. You can optionally add ZORDER BY to physically order data by specific columns, which improves query performance on those columns. OPTIMIZE is not automatic but should run periodically.
VACUUM: Cleaning Up Old Files
When you delete rows or overwrite partitions, old Parquet files are no longer referenced by the Delta log but still exist on disk. VACUUM removes these orphaned files. The default retention window is 7 days (so you can still time travel). Setting retention too low can break time travel queries. VACUUM reduces storage costs but requires careful planning.
Temporary and Global Temp Views
Temp views are session scoped, meaning they exist only for your current session or notebook run. Global temp views live in the global_temp schema and can be accessed across notebooks running on the same cluster. Neither type creates actual table metadata in the metastore. They're useful for intermediate transformations and data exploration.
DESCRIBE HISTORY: Tracking Table Versions
DESCRIBE HISTORY shows every transaction on a Delta table: version number, timestamp, operation (INSERT, UPDATE, DELETE, MERGE, etc.), user who made the change, and more. This is invaluable for auditing and understanding what happened to your data.
INSERT OVERWRITE vs INSERT INTO
INSERT INTO appends new rows. INSERT OVERWRITE replaces all data in the table (or partitions if specified). For partitioned tables, you can overwrite specific partitions only. INSERT OVERWRITE is useful for periodic batch loads where you know the full dataset for a time period.
---
## Code Examples
**MERGE INTO: The Classic Upsert Pattern**
```sql
MERGE INTO customers t
USING new_customers s
ON t.customer_id = s.customer_id
WHEN MATCHED AND s.is_deleted = true THEN DELETE
WHEN MATCHED THEN UPDATE SET *
WHEN NOT MATCHED THEN INSERT *
```
This statement handles three cases: if a customer exists and is marked deleted, remove it; if they exist, update their record; if they don't exist, insert them. The * syntax matches all columns automatically.
**Time Travel Queries**
```sql
-- Query data at version 5
SELECT * FROM orders VERSION AS OF 5;
-- Query data from a specific timestamp
SELECT * FROM orders TIMESTAMP AS OF '2025-01-01 10:00:00';
-- Restore table to version 3
RESTORE TABLE orders TO VERSION AS OF 3;
```
These queries let you debug data issues by seeing what the table looked like in the past. RESTORE is useful if you accidentally deleted important data.
**OPTIMIZE and VACUUM**
```sql
-- Compact small files into larger ones
OPTIMIZE sales_data;
-- Optimize and Z-order by region for faster region-based queries
OPTIMIZE sales_data ZORDER BY (region);
-- Remove orphaned files older than 7 days (default)
VACUUM sales_data;
-- Remove files older than 30 days (requires change)
VACUUM sales_data RETAIN 30 DAYS;
```
**CREATE OR REPLACE TABLE (CRAS)**
```sql
-- Rebuild gold table atomically
CREATE OR REPLACE TABLE gold.customer_summary AS
SELECT 
  customer_id,
  COUNT(*) as order_count,
  SUM(amount) as total_spent
FROM silver.orders
GROUP BY customer_id;
```
**Temporary and Global Temp Views**
```sql
-- Create a temp view (session scoped)
CREATE OR REPLACE TEMP VIEW customer_purchases AS
SELECT customer_id, COUNT(*) as purchases
FROM orders
GROUP BY customer_id;
-- Create a global temp view (accessible across notebooks on same cluster)
CREATE OR REPLACE GLOBAL TEMP VIEW recent_orders AS
SELECT * FROM orders
WHERE order_date >= current_date() - 7;
-- Access global temp view
SELECT * FROM global_temp.recent_orders;
```
**DESCRIBE HISTORY**
```sql
-- View table version history
DESCRIBE HISTORY customers;
-- Results show: version, timestamp, user_id, operation, etc.
-- Example row:
-- version=5, timestamp=2025-01-15 14:30:00, operation=MERGE, 
-- user=analyst@company.com
```
---
## Common Exam Scenarios
**Scenario 1: Handling Late Arriving Dimensions**
You're loading customer data daily from an upstream system. Some updates arrive days late (a customer changes their address). You can't use INSERT OVERWRITE because you'd lose rows that arrived early. You need to handle updates to existing customers and inserts for new ones in a single operation. Answer: Use MERGE INTO to match on customer_id. If matched, update all columns. If not matched, insert the new row. This ensures your customers table stays current and handles late arrivals gracefully.
**Scenario 2: Recovering from a Data Pipeline Error**
Yesterday morning at 9 AM your data pipeline ran but had a bug that corrupted the gold orders table. You've already fixed the code, but you need to recover the table to its state before the bad run. You know DESCRIBE HISTORY can show you the version numbers and timestamps. Answer: Query DESCRIBE HISTORY to find the version just before 9 AM. Then use RESTORE TABLE orders TO VERSION AS OF <version_number>. The table is restored atomically and you can safely rerun today's load.
**Scenario 3: Slow Queries on a Large Table**
Your transactions table has grown to billions of rows. Queries that filter by region take longer than expected. You notice the table has many small files (result of 100 daily inserts). Answer: Run OPTIMIZE transactions ZORDER BY (region) to compact files and physically organize data by region. This reduces the number of files Spark has to read and allows Z-order clustering to skip entire files based on the region predicate. Queries improve significantly.
---
## Key Takeaways
- MERGE INTO is the primary upsert pattern in Databricks. It's atomic and handles INSERT, UPDATE, and DELETE in one statement. Master the syntax and when to use each WHEN clause.
- Time travel is a Delta Lake superpower. Query data at any point in time using VERSION AS OF or TIMESTAMP AS OF. Use RESTORE to recover the entire table to a previous state.
- OPTIMIZE and VACUUM are maintenance operations. OPTIMIZE compacts files and improves query performance. VACUUM removes orphaned files and reduces storage. Always understand the retention window before running VACUUM.
- CREATE OR REPLACE TABLE is atomic and preserves table history unlike DROP and recreate. Use it for periodic full rebuilds of summary tables.
- Temporary views are session scoped and useful for intermediate work. Global temp views live in the global_temp schema and work across notebooks on the same cluster.
---
## Gotchas and Tips
VACUUM breaks time travel. If you VACUUM with a short retention window (like 1 day), you can no longer query old versions or restore beyond that window. Always understand the tradeoff between storage savings and historical data access before running VACUUM.
INSERT INTO is append only, INSERT OVERWRITE replaces. It's easy to mix these up. If you need to reload historical data, use INSERT OVERWRITE on the target partition or table. If you're appending new data, use INSERT INTO.
Temp views don't create metadata. They're only available in your session and don't show up in SHOW TABLES (unless you filter for temp tables). Global temp views persist across notebooks but only for the cluster lifetime. When the cluster terminates, they vanish.
---
## Links and Resources
- **[SQL language reference](https://docs.databricks.com/aws/en/sql/language-manual/)** — Complete SQL reference for Databricks including all DDL and DML statements.
- **[Upsert into a Delta Lake table using merge](https://docs.databricks.com/aws/en/delta/merge)** — MERGE INTO syntax, schema evolution, and advanced use cases.
- **[Update table schema](https://docs.databricks.com/aws/en/delta/update-schema)** — How to add, rename, and drop columns using DDL and DML.