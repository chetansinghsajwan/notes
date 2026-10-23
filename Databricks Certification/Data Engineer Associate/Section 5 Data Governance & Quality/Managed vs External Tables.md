## Topic Overview
In Unity Catalog, tables come in two types: **Managed tables** and **External tables**. Each serves different needs depending on your data governance and storage strategy.
Managed tables are the default and recommended approach. Unity Catalog manages both the metadata (the schema, table definition) and the underlying data files. Data is stored in the managed storage location that you configure for the catalog or schema. When you DROP a managed table, both the metadata and the actual data files are deleted from cloud storage. This means Databricks owns the complete lifecycle of the table.
External tables take a different approach. Unity Catalog manages only the metadata. The actual data files live at a user specified location in cloud storage (S3, Azure Storage, GCS, etc.). When you DROP an external table, only the metadata is removed from Unity Catalog. The data files stay behind in cloud storage. This is useful when data is shared with systems outside Databricks or when the data lifecycle is controlled by external processes.
The choice between managed and external tables often comes down to this: use managed tables when Databricks is the primary owner and operator. Use external tables when the data lives elsewhere or needs independence from Databricks lifecycle management.
---
## Key Concepts
**Managed Tables**
Unity Catalog controls the entire table lifecycle. Data files are stored in the managed location (default storage path for the catalog or schema). Metadata and data are in sync and deleted together. Eligible for automatic optimization by Databricks (file compaction, clustering, etc.). Easier to manage because you don't have to worry about storage paths or data cleanup.
**External Tables**
Unity Catalog manages only the metadata. Data files live at a specific cloud storage path that you specify. Dropping the table removes metadata but leaves data in place. Requires an External Location and Storage Credential to be configured in Unity Catalog for governance. Useful for data shared with external systems, federated analytics, or when data lifecycle is managed outside Databricks.
**When to Use Each**
Use Managed tables by default. They are simpler to operate, benefit from Databricks optimizations, and provide complete governance. Use External tables when: data is shared with non-Databricks systems, you need explicit control over storage location, data is managed by an external ETL pipeline, or you want to decouple the table lifecycle from storage.
**Managed Storage Locations**
Each catalog and schema can have a default storage location configured. When you create a managed table without specifying a LOCATION, it automatically stores data in the catalog or schema's managed location. Managed locations are cloud storage paths (like s3://my-bucket/uc/catalogs/) that Databricks uses to store data for managed tables. You configure these at the catalog and schema level for organizational purposes.
**External Locations and Storage Credentials**
External tables require two Unity Catalog objects: an External Location (a reference to a cloud storage path) and a Storage Credential (credentials to access that path). Together they provide governance and access control. You create these once and then reference them when creating external tables.
**Volumes vs Tables**
Don't confuse tables with Volumes. Volumes are for storing unstructured or semi structured data (images, PDFs, raw files). Tables are for structured data with schemas. Both managed and external tables store structured data. Volumes provide lower level file storage access but without table semantics.
---
## Code Examples
Creating a Managed Table (default behavior):
```sql
CREATE TABLE my_catalog.my_schema.sales (
    order_id INT,
    customer_id INT,
    amount DECIMAL(10, 2),
    order_date DATE
);
```
Creating an External Table with a specific location:
```sql
CREATE TABLE my_catalog.my_schema.raw_events
LOCATION 's3://my-data-bucket/events/'
AS SELECT * FROM parquet.`s3://my-data-bucket/events/`;
```
Inspecting table details to see if it's managed or external:
```sql
DESCRIBE DETAIL my_catalog.my_schema.sales;
-- Look for: table_type (MANAGED or EXTERNAL), location, etc.
```
Checking the properties of a table:
```sql
SHOW TBLPROPERTIES my_catalog.my_schema.sales;
-- Shows metadata including table type and storage details
```
Comparing DROP behavior:
```sql
-- Managed table: DROP removes metadata AND data files
DROP TABLE my_catalog.my_schema.sales;
-- External table: DROP removes metadata ONLY
DROP TABLE my_catalog.my_schema.raw_events;
-- Data files at s3://my-data-bucket/events/ still exist
```
---
## Common Exam Scenarios
**Scenario 1: Data Cleanup After Table Drop**
You have a production table storing customer orders in Unity Catalog. A colleague accidentally creates an external table pointing to a shared S3 bucket containing data from multiple systems. They drop the external table, but the data in the S3 bucket is still needed by another analytics platform. What happened? The external table metadata was deleted, but the underlying data files remained in S3. If this had been a managed table, both the metadata and files would be gone, breaking the other system. This demonstrates why external tables are preferable when data has external dependencies.
**Scenario 2: Choosing Table Type for a Data Lake**
You're building a medallion architecture (Bronze/Silver/Gold) in Unity Catalog. Bronze tables ingest raw data from APIs. Silver tables contain cleaned, deduplicated data. Gold tables serve business analytics. Which table type should you use? For all three layers, use managed tables. Databricks will handle storage, optimization, and lifecycle management. You only need external tables if data needs to live outside Databricks or be accessed by non-Databricks systems. Since you control the entire pipeline, managed is the right choice.
**Scenario 3: External Location Governance**
Your organization needs to grant team members permission to query data in an external S3 location, but you don't want them to access the raw S3 bucket directly. You create an External Location and Storage Credential in Unity Catalog, then create an external table pointing to that location. Team members get permission only on the external table, not the S3 bucket. This centralizes governance and ensures access is tracked through Unity Catalog audit logs, not raw cloud storage logs.
---
## Key Takeaways
- Managed tables are the default and recommended choice. Unity Catalog manages both metadata and data. Use them unless you have a specific reason to use external tables.
- External tables store metadata in Unity Catalog but data lives in cloud storage. Dropping an external table only removes metadata, not the underlying data files.
- Use `CREATE TABLE` for managed tables. Use `CREATE TABLE ... LOCATION 'path'` for external tables.
- External tables require External Locations and Storage Credentials configured in Unity Catalog for governance and access control.
- Use `DESCRIBE DETAIL table_name` to check whether a table is managed or external and see its storage location.
---
## Gotchas and Tips
Dropping an external table does NOT delete the underlying data. If you DROP an external table and then need to recreate it, the data is still there. You can point a new external table to the same location and recover the schema by re reading the data. With managed tables, data is gone forever after DROP.
Don't confuse managed storage locations with External Locations. Managed storage locations are where Databricks stores managed table data automatically. External Locations are user defined references to cloud storage paths for external tables. You need both concepts to understand the full picture.
External tables require explicit configuration of External Locations and Storage Credentials. If these are not set up, you cannot create external tables. Managed tables have zero external setup because Databricks provides the storage automatically. This is one reason managed tables are easier to use.
---
## Links and Resources
- **[Unity Catalog managed tables](https://docs.databricks.com/aws/en/tables/managed)** — Benefits of managed tables including automatic optimization and governance.
- **[Work with external tables](https://docs.databricks.com/aws/en/tables/external)** — Creating and managing external tables with Unity Catalog governance.
- **[Convert an external table to a managed table](https://docs.databricks.com/aws/en/tables/convert-external-managed)** — How to migrate from external to managed using SET MANAGED.