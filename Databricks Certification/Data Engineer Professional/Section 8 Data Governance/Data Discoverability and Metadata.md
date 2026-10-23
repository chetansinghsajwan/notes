## Topic Overview
Data discoverability and metadata management are foundational to building trust in the Lakehouse. When you have hundreds of tables and external data sources, users need a way to understand what data exists, what it means, and how it relates to other assets. Unity Catalog (UC) provides a centralized metadata layer that makes this possible.
As a Data Engineer, you need to document your data products, make them searchable, and help analysts discover tables. This includes tagging for business alignment, writing column level documentation, and querying the catalog using INFORMATION_SCHEMA to understand data lineage and dependencies.
## Key Concepts
**Tagging and Comments in Unity Catalog**
UC supports custom tags (key value pairs) on tables, columns, and schemas. Use tags to mark tables by business domain, sensitivity level (PII, confidential, public), or data quality tier (bronze, silver, gold). Comments explain the business purpose, valid ranges, and update frequency. Comments appear in the catalog UI and help analysts understand data without external documentation.
**INFORMATION_SCHEMA for Programmatic Access**
The system.information_schema schema provides standard SQL views for querying metadata. Query tables, columns, schemata, or catalogs to discover by name, owner, catalog, or schema. Use INFORMATION_SCHEMA in lineage jobs to identify all upstream and downstream dependencies without relying on the UI.
**Data Lineage for Discovery and Impact Analysis**
UC automatically tracks read and write relationships between tables. The lineage view shows upstream sources and downstream consumers. Use lineage to identify which tables feed into a dimension or what downstream tables depend on a source. During troubleshooting, trace which downstream tables are affected if data quality drops.
**Searching and Browsing the Catalog UI**
The UC catalog UI provides a searchable browser for metastore objects. Search by table name, column name, owner, or tag. Full text search includes comments, so well documented tables are more discoverable. Browse the schema hierarchy and view column level metadata. The UI is the primary discovery mechanism for most analysts.
## Code Examples
### Adding Comments and Tags
```sql
-- Add table comment
COMMENT ON TABLE my_catalog.my_schema.customers IS
  'Master customer dimension. Updated daily at 2 AM UTC.';
-- Add column comment
COMMENT ON COLUMN my_catalog.my_schema.customers.email IS
  'Customer email address. Primary contact method.';
-- Tag a table
ALTER TABLE my_catalog.my_schema.customers
SET TAGS ('business_unit' = 'sales', 'sensitivity' = 'pii');
-- Tag a column
ALTER TABLE my_catalog.my_schema.customers
ALTER COLUMN ssn SET TAGS ('pii' = 'yes');
```
### Querying INFORMATION_SCHEMA
```sql
-- Find tables by name
SELECT table_catalog, table_schema, table_name
FROM system.information_schema.tables
WHERE table_catalog = 'my_catalog'
  AND table_name LIKE '%customer%';
-- Get column metadata
SELECT column_name, data_type, is_nullable, column_comment
FROM system.information_schema.columns
WHERE table_catalog = 'my_catalog'
  AND table_schema = 'my_schema'
  AND table_name = 'customers'
ORDER BY ordinal_position;
-- Find tables by owner
SELECT table_catalog, table_schema, table_name, table_owner
FROM system.information_schema.tables
WHERE table_owner = 'analytics@company.com'
ORDER BY table_schema;
```
## Common Exam Scenarios
**Cannot discover a newly created table. You documented it with comments but no one can find it in the catalog. What approach would you take?**
Tag the table with a business domain tag and ensure the owner is correct. Comments are searchable via the catalog UI full text search, but tags make the table filterable. Add a tag like domain = features so analysts can browse by domain. Ensure the table is in a schema that analysts typically access.
**Audit which downstream tables depend on a critical raw data source before making schema changes. How would you gather this programmatically?**
Query system.access.audit_logs to see which jobs and notebooks read from the source table. UC automatically tracks table dependencies, allowing you to fetch this information via API. This shows exactly which tables, jobs, or dashboards would be affected by schema changes.
## Key Takeaways
- Use comments and tags strategically for governance and discoverability.
- INFORMATION_SCHEMA is your programmatic gateway to metadata.
- Lineage is automatic in UC. Use it for impact analysis.
- Well documented, tagged tables are more discoverable.
## Gotchas and Tips
Comments are not searchable in INFORMATION_SCHEMA queries. The catalog UI searches comments, but SQL queries do not. Use tags for programmatic discovery.
Lineage only tracks UC tables. If your pipeline mixes UC and non UC tables, you lose visibility into some dependencies.
---
## Links and Resources
- **[Unity Catalog Overview](https://docs.databricks.com/aws/en/data-governance/unity-catalog/)** — Data discoverability through tags, comments, and metadata management in Unity Catalog.
- **[Data Lineage](https://docs.databricks.com/aws/en/data-governance/unity-catalog/data-lineage)** — Tracking data provenance and metadata across the entire data lifecycle.