## Topic Overview
Delta Sharing is an open protocol that lets you securely share live data between organizations without ever copying it. Instead of ETL pipelines that duplicate data and cost money, the provider keeps the data in their own storage and the recipient queries it directly in real time. There are two flavors: Databricks to Databricks (D2D) sharing, which works natively through Unity Catalog, and Databricks to Open (D2O) sharing, which uses the open Delta Sharing protocol so recipients don't need Databricks. You share data via a Share object, which is basically a named collection of tables or views. You define who can receive it with a Recipient object, and the organization sharing the data is the Provider.
The key benefit is that data stays in the provider's storage and recipients always get live, up to date data. For D2D sharing, Unity Catalog handles governance directly. For D2O sharing, the recipient uses a bearer token to authenticate and can access data with Spark, pandas, Power BI, Tableau, or other tools that understand the Delta Sharing protocol. Recipients always get read only access—they can query the data but cannot modify it.
---
## Key Concepts
What is Delta Sharing
An open protocol developed by Databricks that allows secure data sharing between organizations without moving or copying data. The recipient accesses live data directly from the provider's storage and pays only for compute to query it. The provider retains full control and can revoke access instantly.
D2D vs D2O Sharing
D2D (Databricks to Databricks): Both provider and recipient use Databricks. Sharing is managed through Unity Catalog with native integration. Governance controls, permissions, and audit logs are handled automatically. D2O (Databricks to Open): The recipient does not need Databricks. They authenticate with a bearer token and access data using the open Delta Sharing protocol via Spark connectors, pandas, Power BI, Tableau, or other tools. D2O is useful for partners, customers, or teams using different analytics platforms.
Shares, Providers, and Recipients
A Share is a named container of tables, views, and schemas you want to share. You create it on the provider side and grant SELECT permissions on specific objects. A Recipient is an identity you create to represent who gets the share. For D2D, a recipient is a Databricks workspace ID or account. For D2O, a recipient is an external user with an email and generated bearer token. The Provider is your organization or workspace that owns the data and creates the share.
Advantages of Delta Sharing
- No ETL needed. No extract, transform, load pipeline required. Recipients access data directly.
- No data duplication. Data stays in the provider's storage. No copying means no extra storage costs for the provider.
- Real time access. Recipients always get the latest data. No stale snapshots.
- Cross platform compatibility. D2O works with Spark, pandas, Power BI, Tableau, and any tool that supports Delta Sharing.
- Instant revocation. Provider can disable access immediately without managing separate copies.
Limitations and Cost Model
Limitations: Recipients cannot modify shared data. Shares are read only. You cannot grant UPDATE, DELETE, or INSERT permissions. Shared data cannot be used to create new shares downstream (no multi level sharing). Cost model: The provider pays for the storage that holds the shared data. The recipient pays for the compute they use to query it. So if you are the provider sharing a 100 GB table, you pay storage costs. The recipient's Databricks cluster or compute service incurs charges when they run queries against it. For D2O recipients, they pay their own tool's compute costs, not Databricks.
---
## Code Examples
Provider side: creating and configuring shares
```sql
-- Create a share
CREATE SHARE customer_data;
-- Add tables to the share
ALTER SHARE customer_data ADD TABLE catalog.schema.customers;
ALTER SHARE customer_data ADD TABLE catalog.schema.orders;
-- You can also add views
ALTER SHARE customer_data ADD VIEW catalog.schema.customer_summary;
```
Provider side: creating recipients and granting shares
```sql
-- Create a D2D recipient (Databricks workspace)
CREATE RECIPIENT partner_workspace USING ID '1234567890-abcdef';
-- Create a D2O recipient (external user or organization)
CREATE RECIPIENT external_partner USING BEARER_TOKEN;
-- This generates a token automatically that you share with the recipient
-- Grant the share to a recipient
GRANT SELECT ON SHARE customer_data TO RECIPIENT partner_workspace;
GRANT SELECT ON SHARE customer_data TO RECIPIENT external_partner;
-- View recipients and shares
SHOW RECIPIENTS;
SHOW SHARES;
```
Recipient side: accessing shared data (D2D with Databricks)
```sql
-- View available shares
SHOW SHARES;
-- Mount a shared table into your workspace
CREATE TABLE my_catalog.my_schema.customers
  USING DELTADB
  LOCATION 'share/provider_workspace/customer_data/customers';
-- Query the shared data
SELECT * FROM my_catalog.my_schema.customers LIMIT 10;
```
Recipient side: accessing shared data (D2O with Spark connector)
```python
# D2O recipient using Spark
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName("DeltaSharingReader").getOrCreate()
# Read shared data using Delta Sharing URL
share_url = "https://sharing.databricks.com/api/2.0/shares/customer_data/objects"
token = "<bearer_token_from_provider>"
df = spark.read.format("deltaSharing") \
    .option("bearerToken", token) \
    .option("shareUrl", share_url) \
    .load("customers")
df.show()
```
---
## Common Exam Scenarios
### Scenario 1: Choosing Between D2D and D2O
Your organization has a data lake on Databricks and needs to share customer analytics data with an external partner who uses Tableau. Which sharing model should you use? D2O. The partner does not have Databricks, but they can authenticate with a bearer token and use the Delta Sharing protocol with Tableau. D2D would not work here because the recipient is not on Databricks. This scenario tests understanding of when each sharing model applies.
### Scenario 2: Cost and Storage Responsibility
Your company shares a 500 GB fact table with five partner organizations via Delta Sharing. Who pays for the storage? Your company (the provider). One of the partners runs a query that scans the entire table. Who pays for that compute? The partner. This tests understanding of the cost model. The provider always pays for storage. Recipients pay for compute.
### Scenario 3: Read Only Constraints in Production
You receive a request from a partner to update a few rows in a shared table for data correction. Is this possible with Delta Sharing? No. Shares are read only. If the partner needs to modify data, you would have to use an alternative approach like a custom API or ETL pipeline that allows write access. This tests understanding that Delta Sharing provides read only access and that recipient data quality requirements must be met by the provider, not the recipient.
---
## Key Takeaways
- Delta Sharing is an open protocol for sharing live data without copying. Providers retain ownership and recipients always access current data.
- D2D (Databricks to Databricks) uses Unity Catalog natively. D2O (Databricks to Open) supports non Databricks recipients via bearer tokens and is cross platform.
- Provider pays for storage. Recipients pay for compute. This makes sharing cost efficient for the provider.
- Shares are read only. Recipients cannot modify shared data. Access can be revoked instantly.
- Use CREATE SHARE to define shares, ALTER SHARE to add objects, CREATE RECIPIENT to define recipients, and GRANT SELECT ON SHARE to enable sharing.
---
## Gotchas and Tips
Shares are always read only. You cannot grant write permissions to recipients. This is fundamental to Delta Sharing and applies to D2D and D2O alike. If a recipient needs to modify data, use a different sharing mechanism.
Bearer tokens for D2O are generated once when you create a recipient. If you lose the token, you cannot retrieve it. Store tokens securely and rotate them periodically for security.
D2O recipients can access data with non Databricks tools, but those tools must support the Delta Sharing protocol. Always verify tool compatibility before sharing.
---
## Links and Resources
- **[Delta Sharing Overview](https://docs.databricks.com/aws/en/delta-sharing/)** — Introduction to the open protocol for secure data sharing.
- **[Share Data with Databricks Recipients](https://docs.databricks.com/aws/en/delta-sharing/share-data-databricks)** — Databricks-to-Databricks sharing setup and management.
- **[Share Data with Open Recipients](https://docs.databricks.com/aws/en/delta-sharing/share-data-open)** — Sharing data with non-Databricks consumers using the open protocol.