A **data warehouse** is a centralized repository that stores large volumes of structured, historical data from multiple sources, optimized for querying and analysis (not transaction processing).
**Key traits:**
- Stores historical data (months to years)
- Read-heavy, not write-heavy
- Data is cleaned and transformed before storage (ETL process)
- Designed for analytics and reporting, not day-to-day operations
**How it differs from a regular database:**
|Aspect|Database (OLTP)|Data Warehouse (OLAP)|
|---|---|---|
|Purpose|Day-to-day operations|Analytics & reporting|
|Data type|Current, transactional|Historical, aggregated|
|Query type|Simple, frequent|Complex, infrequent|
|Schema|Normalized|Denormalized (star/snowflake)|
|Speed optimized for|Writes|Reads|
**Popular tools:**
|Tool|Type|Approx. cost (INR/TB/month)|
|---|---|---|
|Google BigQuery|Cloud|₹1,700–₹2,100|
|Amazon Redshift|Cloud|₹7,000–₹15,000|
|Snowflake|Cloud|₹7,000–₹21,000|
|Azure Synapse|Cloud|₹6,000–₹12,000|
|**Databricks**|Cloud/Lakehouse|Usage-based (DBU pricing)|
Given your Databricks background, you're likely already working in a **Lakehouse** architecture — which blends data warehouse capabilities with a data lake (Delta Lake being the storage layer).