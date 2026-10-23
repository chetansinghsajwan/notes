## Topic Overview
Lakehouse Federation is a feature that lets you query external databases directly from Databricks without copying the data over. Think of it as creating a bridge to databases like MySQL, PostgreSQL, SQL Server, Snowflake, BigQuery, or Redshift. You register these external systems using **Connections** and **Foreign Catalogs** in Unity Catalog, then query them using familiar SQL and the same `catalog.schema.table` syntax you use for local tables.
The power here is **query pushdown**. When you run a query against a federated table, Databricks sends the filtering and computation down to the source database instead of pulling raw data and processing it locally. This keeps your data where it lives while giving you a unified query experience across all your data sources.
Lakehouse Federation is read only. You can't write back to federated tables directly. It's best for scenarios where you need to join your Databricks lakehouse with live operational data, gradually migrate off legacy systems, or access reference tables stored elsewhere.
---
## Key Concepts
What is Lakehouse Federation?
Lakehouse Federation enables you to query data stored in external databases from within Databricks without moving or replicating that data. It's achieved through Foreign Catalogs registered with Unity Catalog. Your queries execute against the remote database, returning results through Databricks. This is particularly useful for accessing live operational data or reference tables while maintaining a single query interface.
Connections
A Connection stores the credentials and metadata needed to connect to an external database. It includes the host, port, database name, username, and password (encrypted in Databricks). Connections are the foundational object. You create them first, then reference them when creating Foreign Catalogs. Multiple Foreign Catalogs can use the same Connection if they point to different databases on the same server.
Foreign Catalogs
A Foreign Catalog is a Unity Catalog that maps to an external database via a Connection. It acts as the entry point for querying remote data. Under a Foreign Catalog, you see schemas and tables that mirror the structure of the external database. You control access to foreign catalogs using UC permissions (USAGE, SELECT, etc.). From the user's perspective, they query a foreign catalog the same way they query a local UC catalog.
Query Pushdown
Query pushdown means that filtering, aggregations, and joins are sent to the remote database to execute, rather than fetching all the raw data to Databricks. This dramatically reduces network traffic and improves performance. Not all SQL operations can be pushed down (complex functions, certain joins), so Databricks intelligently decides what to push and what to compute locally. The remote database does the heavy lifting.
Benefits of Lakehouse Federation
- No data movement: keep data where it lives, query it from Databricks
- Unified governance through Unity Catalog controls
- Single query engine across multiple data sources
- Access live data without replication or ETL pipelines
- Efficient joins between lakehouse and operational data
Limitations of Lakehouse Federation
- Read only: no INSERT, UPDATE, DELETE operations to federated tables
- Performance depends on the remote database and network latency
- Not all SQL features are supported (some functions, data types)
- Remote system must be accessible and available
---
## Code Examples
Creating a Connection to a PostgreSQL database:
```sql
CREATE CONNECTION postgres_prod
PROVIDER = postgresql
OPTIONS (
  host = 'prod-db.example.com',
  port = 5432,
  user = 'databricks_user',
  password = 'secure_password'
);
```
Creating a Foreign Catalog from that Connection:
```sql
CREATE FOREIGN CATALOG postgres_external
USING CONNECTION postgres_prod
DATABASE production;
```
Querying a federated table (same syntax as local tables):
```sql
SELECT customer_id, order_count
FROM postgres_external.public.customers
WHERE country = 'USA'
LIMIT 1000;
```
Joining lakehouse data with federated tables:
```sql
SELECT 
  l.transaction_id,
  l.amount,
  f.customer_name,
  f.country
FROM main.analytics.transactions l
INNER JOIN postgres_external.public.customers f
  ON l.customer_id = f.customer_id
WHERE l.transaction_date >= '2025-01-01';
```
---
## Common Exam Scenarios
Scenario 1: You work for a fintech company with customer accounts in PostgreSQL and transaction data in Databricks. You need to join these datasets to analyze spending patterns without moving production data. You create a Connection to your PostgreSQL cluster, then a Foreign Catalog mapped to the accounts database. Queries against the foreign catalog tables use predicate pushdown to filter accounts server side, returning only matching rows to Databricks for the join. This minimizes data transfer and doesn't disrupt your operational database performance.
Scenario 2: Your company is migrating from Snowflake to Databricks but has legacy reference tables still on Snowflake (SKUs, product hierarchies, pricing). Instead of copying all this data, you use Lakehouse Federation to query the Snowflake reference tables directly. Analytics teams can join your Databricks lakehouse fact tables with federated reference tables. Once migration is complete, you simply drop the foreign catalogs. This approach lets you migrate gradually without waiting for all reference data to move.
Scenario 3: An exam question asks which approach is best for querying live operational data from MySQL without moving it: (A) ETL pipeline to replicate data daily, (B) Lakehouse Federation with Foreign Catalog, (C) export to CSV and upload. The answer is (B). Federation lets you query current data without pipelines or ETL overhead, and Unity Catalog controls access. The key insight is that federation is read only, so it's perfect for reference and operational queries, not for storing your transformed data.
---
## Key Takeaways
- Lakehouse Federation lets you query external databases (PostgreSQL, Snowflake, BigQuery, etc.) directly from Databricks without copying data
- Use **Connections** to store connection credentials and **Foreign Catalogs** to map external databases into Unity Catalog
- Query pushdown sends filtering and computation to the source database, reducing network traffic and improving performance
- Federation is read only. You cannot INSERT, UPDATE, or DELETE through federated tables
- Perfect use cases: joining lakehouse data with live operational systems, gradual migration from legacy platforms, accessing reference data without replication
---
## Gotchas and Tips
Lakehouse Federation is read only. If a question asks about updating data in a federated table, the answer is no. You must use an ETL pipeline to move and transform the data into your lakehouse instead.
Performance depends on the remote database. If your source system is slow or under heavy load, federated queries will be slow too. Federation doesn't improve the performance of bad source queries. It just lets you query them from Databricks.
Not all SQL features are supported in pushdown. Complex UDFs, regex, and some window functions may not push down. Databricks will compute these locally after fetching data. Write queries with simple WHERE and JOIN conditions to maximize pushdown efficiency.
---
## Links and Resources
- **[Lakehouse Federation Overview](https://docs.databricks.com/aws/en/query-federation/)** — Querying data in external systems without moving or copying it.
- **[Federation Connections](https://docs.databricks.com/aws/en/query-federation/connections)** — Creating and managing connections to external data sources.