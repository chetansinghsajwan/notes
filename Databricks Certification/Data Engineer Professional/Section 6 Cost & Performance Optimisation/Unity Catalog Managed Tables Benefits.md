## Topic Overview
Unity Catalog managed tables represent a fundamental shift in how you manage data in Databricks. When you create a managed table in Unity Catalog, Databricks takes ownership of the underlying files, handling their lifecycle automatically. This is different from external tables where you manage the files yourself.
The benefits go beyond convenience. Managed tables give you automatic file compaction, streamlined drop operations, and integrated governance. You also get uniform access control, data lineage tracking, and the ability to enforce retention policies at the Catalog level. For organizations dealing with compliance requirements like GDPR or HIPAA, this centralized approach to data management significantly reduces operational overhead.
When you drop a managed table, the data is deleted automatically. This is powerful for compliance but also means you need to understand the implications. Unlike external tables where files linger, managed tables immediately remove data from storage when you drop them. This matters when you need to demonstrate data deletion for audits.
---
## Key Concepts
Managed vs External Tables
Managed tables store data in a location managed by Databricks, typically in Unity Catalog's managed storage. When you drop a managed table, the underlying files are deleted automatically. External tables point to data you manage in cloud storage like S3 or ADLS. You can drop an external table without deleting the data files.
Automatic File Lifecycle Management
Databricks automatically handles file compaction and cleanup for managed tables. The system merges small files, removes unused deletion vectors, and optimizes the file layout without requiring manual optimization commands. This reduces the operational burden on your data engineers.
PURGE on DROP
Dropping a managed table with PURGE immediately and permanently deletes the data. Without PURGE, the table metadata is removed but data remains recoverable briefly. This is essential for compliance scenarios where you need immediate deletion guarantees.
Unified Governance
All managed tables in Unity Catalog inherit consistent access controls, audit logging, and data lineage tracking. You define permissions once at the schema or catalog level, and all tables comply with the same policies. This eliminates the fragmentation you get with external tables scattered across different cloud storage locations.
---
## Code Examples
### Creating Managed Tables in Unity Catalog
```sql
-- Managed table (Databricks manages the files)
CREATE TABLE my_catalog.my_schema.customers (
  customer_id INT,
  name STRING,
  email STRING,
  created_at TIMESTAMP
)
USING DELTA;
-- External table (you manage the files)
CREATE EXTERNAL TABLE my_catalog.my_schema.raw_events (
  event_id INT,
  event_type STRING,
  timestamp TIMESTAMP
)
USING DELTA
LOCATION 's3://my-bucket/events/';
```
### Dropping with PURGE
```sql
-- Immediately delete managed table data (GDPR deletion)
DROP TABLE my_catalog.my_schema.customers PURGE;
-- External table drop (data remains in S3)
DROP TABLE my_catalog.my_schema.raw_events;
```
### Setting Retention Policies
```sql
-- Set retention policy on a managed table
ALTER TABLE my_catalog.my_schema.transactions
SET TBLPROPERTIES ('delta.logRetentionDuration' = '30 days');
-- Enable automatic VACUUM runs
ALTER TABLE my_catalog.my_schema.transactions
SET TBLPROPERTIES ('delta.enableDeletionVectors' = 'true');
```
---
## Common Exam Scenarios
Scenario 1: GDPR Right to Be Forgotten
A company stores customer PII in Unity Catalog managed tables. When a customer requests deletion under GDPR, you need to ensure data is removed immediately and permanently. Using DROP TABLE with PURGE is the right approach because it immediately deletes files and generates an audit log entry proving deletion. External tables would require manual deletion of S3 files, making compliance harder to demonstrate.
Scenario 2: Reducing Storage Costs for Time Series Data
A data platform stores 24 months of clickstream data in managed tables. You want to keep only the last 12 months and delete older partitions. Managed tables with automatic lifecycle management allow you to drop old partitions and immediately free storage. External tables would require you to manually delete files from cloud storage, increasing operational complexity.
Scenario 3: Cross Team Data Governance
Multiple teams share a Unity Catalog with managed tables. Instead of managing separate access control systems for each cloud storage location, you use unified permissions at the catalog and schema level. This ensures consistent governance and audit trails across all teams. New team members automatically inherit correct permissions without manual S3 policy updates.
---
## Key Takeaways
- Managed tables in UC reduce operational overhead by automatically handling file lifecycle, compaction, and cleanup
- Use DROP TABLE with PURGE for immediate data deletion required by GDPR, HIPAA, and other compliance frameworks
- Unified access control and audit logging across all managed tables eliminates governance fragmentation
- Managed tables enforce consistent retention policies at the catalog level, simplifying data lifecycle management
---
## Gotchas and Tips
PURGE is Permanent: DROP TABLE with PURGE permanently deletes data. There is no trash or recycle bin for managed tables. Test your deletion logic on non production tables first.
Storage Location: Managed table files are stored in Unity Catalog's managed storage location, not your own cloud account. You cannot access these files directly via cloud storage APIs.
Migration from External: Migrating from external tables to managed tables requires copying data and updating all downstream dependencies. Plan this migration carefully in your Lakeflow pipelines.
---
## Links and Resources
- **[Managed Tables](https://docs.databricks.com/aws/en/tables/managed)** — Benefits of Unity Catalog managed tables for governance and lifecycle management.
- **[Unity Catalog Overview](https://docs.databricks.com/aws/en/data-governance/unity-catalog/)** — Centralized governance model and its cost optimization benefits.