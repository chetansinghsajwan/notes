## Topic Overview
Lakehouse Federation lets you query data from external databases directly in Databricks without moving it into Delta Lake. You can connect to PostgreSQL, MySQL, SQL Server, Snowflake, BigQuery, and other sources through foreign catalogs. The key idea is that Databricks acts as a query engine that can push work down to the remote database, so you get federation with governance through Unity Catalog. It's perfect when you have existing data warehouses or operational databases that you want to query alongside your data lake.
To set up federation, you create a connection object with credentials to the external database, then create a foreign catalog that references that connection. The foreign catalog appears in your Unity Catalog hierarchy just like a normal catalog. When you query a table in a foreign catalog, Databricks optimizes the query and pushes it to the remote database when possible. Access is governed by the same UC permissions model, so you get consistent access control across all your data.
## Key Concepts
**Connections and Foreign Catalogs**
A connection stores credentials and connection details for an external database. A foreign catalog is a named reference to that connection, which creates a catalog in Unity Catalog that maps to the external database. Tables in foreign catalogs are read only. The foreign catalog uses the connection to authenticate and query the remote database. You can have multiple foreign catalogs pointing to the same connection, or pointing to different databases via the same connection.
**Query Pushdown and Optimization**
Query pushdown means that when you execute a query on a foreign table, Databricks translates the SQL and sends it to the remote database to execute, rather than pulling all the data to Spark. This minimizes data movement and leverages the remote database's query engine for efficiency. Not all queries can be pushed down. Queries with Spark specific functions or very complex logic may need to be executed in Spark on the remote data. The Databricks query planner handles this automatically.
**Unity Catalog Governance on Federated Data**
Foreign catalogs inherit the same UC permission model. You can grant catalog level permissions, schema level permissions, and object level permissions just like on Delta Lake. Row filters and column masks apply to federated queries too. This means your data security policies are consistent whether data lives in your data lake or in an external system.
**Supported Data Sources**
Lakehouse Federation supports PostgreSQL, MySQL, MariaDB, SQL Server, Snowflake, BigQuery, and Azure Data Warehouse. Each has specific connection parameters and driver requirements. Query pushdown compatibility varies by database. For example, Snowflake shares many SQL functions with Spark, so pushdown is generally effective. PostgreSQL also has good pushdown support.
---
## Code Examples
### Setting Up a Connection and Foreign Catalog (SQL)
```sql
-- Create a connection to a PostgreSQL database
CREATE CONNECTION postgres_prod
FOR DATABASE DRIVER 'postgresql'
WITH (
  host = 'my-postgres-server.example.com',
  port = '5432',
  database = 'analytics_db',
  user = 'databricks_user',
  password = '{{ secrets/scope/postgres_password }}'
);
-- Create a foreign catalog using the connection
CREATE FOREIGN CATALOG postgres_catalog
  USING CONNECTION postgres_prod
  PROPERTY 'schema_mapping' = 'public';
-- Query a table from the foreign catalog
SELECT * FROM postgres_catalog.public.customers LIMIT 10;
-- Create a Snowflake connection with network binding
CREATE CONNECTION snowflake_prod
FOR DATABASE DRIVER 'snowflake'
WITH (
  account = 'xy12345.us-east-1',
  warehouse = 'compute_wh',
  database = 'analytics_db',
  schema = 'public',
  user = 'databricks_user',
  password = '{{ secrets/scope/snowflake_password }}'
);
```
### Querying Federated Data in PySpark
```python
# Read from a foreign catalog table
df = spark.read.table("postgres_catalog.public.orders")
# Filter and transform
filtered = df.filter(col("order_date") > "2024-01-01")
result = filtered.groupBy("customer_id").agg({"total": "sum"})
# Join across local and foreign data
gold_customers = spark.read.table("catalog.gold.customers")
join_result = result.join(
  gold_customers,
  result.customer_id == gold_customers.customer_id,
  "inner"
)
# Collect results to Databricks
join_result.write.mode("overwrite").saveAsTable("catalog.gold.order_summary")
```
### Monitoring Query Pushdown
```sql
-- Check if queries are being pushed down
-- Use EXPLAIN to see the execution plan
EXPLAIN SELECT 
  customer_id, 
  COUNT(*) as order_count
FROM postgres_catalog.public.orders
WHERE order_date > '2024-01-01'
GROUP BY customer_id;
-- Look for "ForeignRelation" or "ForeignScan" in the output
-- This indicates the query is being pushed to the remote database
-- Query Spark system tables for execution statistics
SELECT 
  query_id,
  total_user_time_ms,
  total_scan_time_ms
FROM system.query_history
WHERE query LIKE '%postgres_catalog%'
ORDER BY start_time DESC
LIMIT 10;
```
---
## Common Exam Scenarios
Scenario: Your company has customer data in a Snowflake data warehouse and transaction data in Databricks. You need to join them for analysis. Should you move the Snowflake data to Databricks, or use Lakehouse Federation?
Lakehouse Federation is the right choice. You create a foreign catalog that connects to Snowflake via a connection object. Queries automatically push down to Snowflake when possible, and you avoid copying data. If the join is simple, Databricks may push the entire query to Snowflake. If the join logic requires Spark, Databricks fetches data from Snowflake and performs the join locally. You maintain single versions of truth in both systems.
Scenario: You have a foreign catalog connected to PostgreSQL. A business analyst needs to read from one schema but not another. How do you enforce this with Unity Catalog?
Grant the analyst SELECT permission on the schema they need, and deny permission on the other schema. Unity Catalog permissions apply to foreign catalogs just like local catalogs. You can grant permissions at the catalog level, schema level, or table level. The analyst sees only the objects they have permission for, regardless of what exists in the external database.
Scenario: Your query on a foreign table is slow. You suspect it's not being pushed down to the remote database. How would you verify this?
Run EXPLAIN on your query and look at the execution plan. If you see "ForeignRelation" or "ForeignScan" nodes, the query is being pushed down. If the plan shows multiple stages and shuffles, it's likely that the query could not be fully pushed and is being executed in Spark. Check system.query_history for timing details. Some queries with non pushable operations (custom UDFs, certain Spark functions) will always execute in Spark.
---
## Key Takeaways
- Lakehouse Federation lets you query external databases (Snowflake, PostgreSQL, BigQuery, etc.) without copying data. Create a connection, then a foreign catalog, and query just like local tables.
- Query pushdown optimizes performance by sending queries to the remote database to execute, minimizing data movement. Use EXPLAIN to verify pushdown is happening.
- Foreign catalogs support full Unity Catalog governance. Permissions, row filters, and column masks work the same way on federated data as on local data.
- Foreign catalog tables are read only. If you need to write data, copy it to a local Delta table or use a separate ingestion process.
---
## Gotchas and Tips
Store connection passwords and credentials in a Databricks secret scope using `{{ secrets/scope/key_name }}` syntax. Never hardcode credentials in queries or notebooks. The connection will reference the secret at runtime.
Some SQL dialects differ between databases. If a query works on the remote database directly but fails when pushed down from Databricks, the syntax may need translation. Consider breaking the query into simpler parts that are more likely to push down.
Network connectivity between Databricks and the external database is critical. If the connection times out or the remote database is unreachable, all queries on that foreign catalog will fail. Test connections and consider network binding to ensure reliable access.
---
## Links and Resources
- **[Lakehouse Federation](https://docs.databricks.com/aws/en/query-federation/)** — Configuring federated queries to external databases without data movement.
- **[Federation Connections](https://docs.databricks.com/aws/en/query-federation/connections)** — Creating and managing connections to external data sources like PostgreSQL, MySQL, and Snowflake.