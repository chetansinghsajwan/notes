## Topic Overview
Delta Sharing is Databricks' open protocol for sharing live data with external organizations without copying it. You've got two main scenarios: Databricks to Databricks (D2D) sharing, where both parties run Databricks, and Databricks to Open (D2O) sharing, where consumers use any tool like Spark, pandas, or Power BI. The key advantage is that the data stays in the provider's storage, and the consumer gets real time access with full governance through Unity Catalog.
On the Databricks side, you create a share object that contains the tables or views you want to distribute. You then add recipients (either Databricks accounts or open protocol consumers) to that share. Once activated, consumers get an activation link or credentials to access the shared data. All access is read only and logged in the provider's audit system, giving you complete visibility over who accessed what and when.
## Key Concepts
**Databricks to Databricks (D2D) Sharing**
D2D sharing is the native path between two Databricks workspaces or organizations. The consumer workspace connects directly to the provider's account using their Databricks identity. This is the simplest approach and offers the tightest integration with Unity Catalog. The provider controls permissions and can revoke access instantly. Consumers see the shared data as if it were their own tables in a catalog.
**Databricks to Open (D2O) and Open Protocol**
D2O sharing uses the open Delta Sharing protocol, which works with any consumer that supports it. This includes Spark clusters, pandas DataFrames, Power BI, Tableau, and other tools. The consumer receives a share credentials file (a signed URL or API token) that grants time limited access to specific tables. The open protocol reads data via HTTPS directly from the provider's cloud storage, so no Databricks workspace is required on the consumer side.
**Shares, Recipients, and Activation Links**
A share is a named object in Unity Catalog that groups tables and views you want to distribute. You add recipients to a share. A recipient is either a Databricks account ID (for D2D) or an email address (for open sharing). When you add a recipient to a share, Databricks generates an activation link that the recipient uses to claim access. Once activated, they get credentials stored in their Databricks account (D2D) or a downloadable JSON file (open sharing).
**Read Only Access and Audit Logging**
All shares grant read only access. Consumers cannot write to or modify shared tables. Every access is logged in the provider's audit system. The `system.access` table records which recipients accessed which shares and when. This gives you complete compliance visibility without needing additional external logging.
---
## Code Examples
### Creating and Managing Shares (SQL)
```sql
-- Create a share
CREATE SHARE sales_data_share COMMENT 'Sales data for external partners';
-- Add tables to the share
ALTER SHARE sales_data_share ADD TABLE analytics.gold_sales;
ALTER SHARE sales_data_share ADD TABLE analytics.gold_customers;
-- Add a recipient (for Databricks to Databricks)
ALTER SHARE sales_data_share ADD RECIPIENT <account-id>;
-- Add a recipient (for open sharing)
ALTER SHARE sales_data_share ADD RECIPIENT partner@externalorg.com;
-- List recipients in a share
SHOW RECIPIENTS IN SHARE sales_data_share;
-- Remove a recipient
ALTER SHARE sales_data_share REMOVE RECIPIENT <account-id>;
```
### Querying Audit Logs for Delta Sharing Access
```sql
-- Query access logs for a specific share
SELECT 
  timestamp,
  user_identity,
  action,
  request.share_name,
  request.object_type,
  request.object_name
FROM system.access
WHERE action = 'SHARE_ACCESS' 
  AND request.share_name = 'sales_data_share'
ORDER BY timestamp DESC;
-- Monitor which recipients access what
SELECT 
  DATE(timestamp) as access_date,
  user_identity as recipient,
  COUNT(*) as access_count
FROM system.access
WHERE action = 'SHARE_ACCESS'
GROUP BY DATE(timestamp), user_identity
ORDER BY access_date DESC, access_count DESC;
```
### Consumer Side: Accessing Shared Data in PySpark
```python
# For Databricks to Databricks sharing, access is seamless
# The shared catalog appears in your catalog list
shared_data = spark.read.table("shared_catalog.schema.table_name")
shared_data.display()
# For open sharing with pandas
import pandas as pd
from delta_sharing import DeltaSharingClient
# Load credentials from the activation link JSON file
client = DeltaSharingClient("/path/to/share_credentials.json")
# List tables in the share
shares = client.list_shares()
for share in shares:
    print(share.name)
# Read a table as pandas DataFrame
df = client.load_as_pandas(share_name, schema_name, table_name)
print(df.head())
```
---
## Common Exam Scenarios
Scenario: Your company needs to share a gold table with an external partner who uses Power BI for reporting. The partner does not have a Databricks account. Which sharing approach would you use?
The answer is Databricks to Open (D2O) sharing. Since the consumer uses Power BI and not Databricks, you need the open Delta Sharing protocol. You create a share, add the partner's email as a recipient, and they get an activation link. Once activated, they download a credentials file that allows Power BI to connect directly via HTTPS. D2D only works when both parties have Databricks workspaces.
Scenario: A manager asks if shared Delta tables can be written to by external recipients. What do you tell them?
Delta Sharing always provides read only access. This is by design and cannot be changed. If the external party needs to write data, they would need to use a different mechanism like a copy of the data in their own storage, or a separate ingestion pipeline. This read only guarantee is one of the core security features.
Scenario: You need to prove compliance by showing that only authorized users accessed a shared table. Where do you look?
Query the `system.access` table with filters for `action = 'SHARE_ACCESS'` and the specific share and table name. This table is managed by Databricks and automatically logs every access attempt. It's the authoritative record for compliance audits and includes timestamp, user identity, and the object accessed.
---
## Key Takeaways
- Delta Sharing is a read only, governed way to share live data. D2D is for Databricks to Databricks. D2O uses the open protocol and works with any tool (Spark, pandas, Power BI, Tableau).
- Shares group tables and views. Recipients are either Databricks account IDs (D2D) or email addresses (open sharing). Activation links grant access and store credentials.
- All access is logged in `system.access`. Use this table to audit who accessed what and when for compliance reporting.
- Open sharing credentials are time limited and can be revoked. Consumers access data via HTTPS directly from the provider's storage, so no Databricks workspace is needed.
---
## Gotchas and Tips
Delta Sharing reads from the provider's object storage (S3, Azure, GCS). If the shared table is in a managed Unity Catalog location, Databricks handles the storage transparently. If it's an external table in an external catalog location, make sure the consumer's cloud identity has read access to that location, or use Databricks managed storage.
Activation links expire. If a consumer loses their credentials, regenerate them by creating a new activation link. For open sharing, you can also regenerate the share credentials JSON file without changing the link.
Sharing a view instead of a table is common. Views can filter or transform the underlying table, so you control exactly what the consumer sees. Remember to grant `SELECT` on the underlying tables if you're sharing a view.
---
## Links and Resources
- **[Delta Sharing Overview](https://docs.databricks.com/aws/en/delta-sharing/)** — Introduction to the open protocol for secure data sharing.
- **[Databricks-to-Databricks Sharing](https://docs.databricks.com/aws/en/delta-sharing/share-data-databricks)** — Sharing live data between Databricks workspaces.
- **[Open Sharing with Non-Databricks Recipients](https://docs.databricks.com/aws/en/delta-sharing/share-data-open)** — Sharing data with external consumers via the open Delta Sharing protocol.