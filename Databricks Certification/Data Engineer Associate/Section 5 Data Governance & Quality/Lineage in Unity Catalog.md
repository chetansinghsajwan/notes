## Topic Overview
Unity Catalog automatically captures data lineage whenever queries run through the system. This means you get a complete record of how data flows between tables, notebooks, and jobs without writing any code to enable it. Lineage operates at two levels: table level tracking shows which tables feed into which other tables, while column level lineage zooms in to show how individual columns are transformed and move between tables. Think of it as a map of your entire data pipeline.
The lineage graph displays both upstream dependencies (where the data originates) and downstream consumers (what depends on this data). This bidirectional view is crucial for impact analysis and root cause troubleshooting. When you need to investigate where bad data came from or predict what will break if you change a table, lineage gives you those answers instantly.
Lineage data is stored in Unity Catalog system tables and accessible via REST APIs, making it programmatic and auditable. The system captures lineage from notebooks, jobs, Lakeflow pipelines, and SQL queries automatically. This is especially valuable for compliance scenarios where you need to prove data provenance and track regulatory requirements.
---
## Key Concepts
What is data lineage?
Data lineage is the complete record of how data moves and transforms through your pipelines. Unity Catalog captures this automatically whenever data is read from or written to a managed table. The system tracks the source tables, target tables, transformations applied, and the code that performed those transformations. This creates a queryable audit trail of your data's journey through the organization.
Table level vs column level lineage
Table level lineage shows relationships between entire tables: which tables as a whole are inputs and which are outputs. Column level lineage goes deeper and tracks individual columns. For example, if you have a source table with customer_id, email, and name, and you create a transformed table that renames customer_id to cust_id and masks the email, column level lineage tracks those specific column transformations. Column lineage is more granular and useful for compliance tracking and understanding field-level dependencies.
Upstream and downstream
Upstream means the source or origin of data. If you select the gold_customers table and view its upstream lineage, you see all the tables that feed into it. Downstream means the consumers or destinations of data. If you view gold_customers downstream lineage, you see all the dashboards, reports, and tables that depend on it. The Catalog Explorer UI displays both directions, allowing you to answer "where does this data come from?" and "what will break if I change this table?"
Impact analysis
Impact analysis uses lineage to predict what will be affected by a change. Before modifying or deleting a table, you can view its downstream lineage to see which jobs, dashboards, or downstream tables depend on it. This prevents breaking changes and allows you to coordinate updates across multiple teams. For example, if you want to rename a column, impact analysis shows you every table and job that references that column.
How lineage is captured
Lineage is captured automatically whenever a query executes that reads from or writes to a Unity Catalog managed table. No configuration is needed. The system extracts lineage from the SQL execution plan, notebook cells, job definitions, and Lakeflow pipeline DAGs. This includes both explicit reads (SELECT FROM) and implicit reads (joins, unions). Lineage is captured whether the query succeeds or fails, giving you complete traceability.
Lineage in Catalog Explorer
Open any table in Catalog Explorer and click the Lineage tab to see a visual graph of upstream and downstream dependencies. The graph is interactive: you can click nodes to drill into related tables, collapse/expand branches, and toggle between table and column level views. The UI shows creation timestamps for lineage records and allows filtering by time range. This visual representation makes it easy to understand complex data relationships at a glance.
---
## Code Examples
Lineage is captured automatically without code, but you can query lineage data programmatically using system tables.
### Viewing lineage via system tables
```sql
-- Query the system.access.table_lineage view to see upstream dependencies
SELECT 
  downstream_table_catalog,
  downstream_table_schema,
  downstream_table_name,
  upstream_table_catalog,
  upstream_table_schema,
  upstream_table_name,
  created_timestamp
FROM system.access.table_lineage
WHERE downstream_table_name = 'gold_customers'
ORDER BY created_timestamp DESC;
```
### Example: Creating lineage through a simple pipeline
```sql
-- Bronze table (raw data)
CREATE OR REPLACE TABLE my_catalog.bronze.customers AS
SELECT * FROM external_system.raw_customers;
-- Silver table (cleaned data)
CREATE OR REPLACE TABLE my_catalog.silver.customers_cleaned AS
SELECT 
  customer_id,
  LOWER(TRIM(email)) as email,
  UPPER(TRIM(name)) as name
FROM my_catalog.bronze.customers
WHERE email IS NOT NULL;
-- Gold table (business ready)
CREATE OR REPLACE TABLE my_catalog.gold.customers_analytics AS
SELECT 
  customer_id,
  email,
  name,
  CURRENT_TIMESTAMP as loaded_at
FROM my_catalog.silver.customers_cleaned;
-- Lineage now shows: 
-- external_system.raw_customers -> bronze.customers -> silver.customers_cleaned -> gold.customers_analytics
```
---
## Common Exam Scenarios
### Scenario 1: Root cause analysis after data quality issue
Your finance team reports incorrect customer revenue totals in a downstream analytics table. You need to trace back to find where the bad data originated. Using Unity Catalog lineage, you view the upstream dependencies of the analytics table and follow the chain backwards through silver and bronze layers. Column level lineage helps you identify that the customer_id column was incorrectly joined in the silver transformation. Without lineage, this investigation would take hours. With it, you pinpoint the issue in minutes. The exam tests your understanding that lineage is automatic and bidirectional, letting you navigate both upstream (to find sources) and downstream (to find impact).
### Scenario 2: Impact analysis before schema change
You want to rename a column in a silver table from old_customer_id to new_customer_id. Before making the change, you check the downstream lineage to see which tables depend on this column. You discover that three gold tables and two Lakeflow pipelines reference this column. Instead of renaming it blindly and breaking everything, you now know you need to coordinate the update across those five downstream assets. The exam expects you to recognize that lineage is the tool for this impact assessment and that it works across notebooks, jobs, and pipelines. Lineage prevents breaking changes by making dependencies visible.
### Scenario 3: Compliance and data provenance
A compliance officer asks you to prove the data lineage for customer PII stored in the gold_customers_pii table as part of a regulatory audit. Using Catalog Explorer, you open the table and generate a lineage report showing the complete chain from raw source data through transformations to the final table. Column level lineage shows which specific columns contain PII and how they were masked or encrypted. You export this lineage graph as evidence of data governance. The exam tests that you know lineage is: automatically captured, visible in the UI, queryable via APIs, and suitable for compliance reporting.
---
## Key Takeaways
- Lineage is automatic. Unity Catalog captures it whenever queries run against managed tables. No configuration needed.
- Lineage works at two levels: table level (high level dependencies) and column level (individual field transformations).
- Use lineage for impact analysis (downstream) and root cause analysis (upstream) when troubleshooting.
- Lineage is visible in Catalog Explorer UI and queryable via system.access.table_lineage system table for programmatic access.
- Lineage is essential for compliance and data governance because it proves data provenance and creates an audit trail.
---
## Gotchas and Tips
Lineage only works with managed tables. External tables and non UC locations are not included in lineage graphs. If your source data sits in external tables or cloud storage without UC registration, lineage will start from the first managed table in your pipeline.
Lineage has retention limits. Unity Catalog keeps lineage data for a limited time (typically 90 days for recent data). If you need to analyze lineage from months ago, plan accordingly. Use system tables to export and archive lineage records if needed for compliance.
Column level lineage may not capture all transformations, especially complex ones involving UDFs, case expressions, or nested operations. Use it for simple column mappings and joins, but for intricate logic, supplement it with code comments and documentation.
---
## Links and Resources
- **[Data Lineage in Unity Catalog](https://docs.databricks.com/aws/en/data-governance/unity-catalog/data-lineage)** — Capturing and viewing column-level and table-level lineage across workloads.
- **[Lineage System Tables](https://docs.databricks.com/aws/en/admin/system-tables/lineage)** — Querying lineage data programmatically via system tables.