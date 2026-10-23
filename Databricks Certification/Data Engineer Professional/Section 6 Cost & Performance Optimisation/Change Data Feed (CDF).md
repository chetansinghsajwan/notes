## Topic Overview
Change Data Feed enables you to capture and propagate only the rows that changed in a Delta table. Instead of reprocessing entire tables during ETL, you can read only the inserts, updates, and deletes that occurred since the last run. This dramatically reduces compute, improves latency, and enables real time data propagation.
CDF is essential for incremental ETL pipelines and Change Data Capture scenarios. You enable it at the table level, then query changes using the table_changes() function. The _change_type column tells you whether each row was inserted, updated, or deleted. This works seamlessly with Lakeflow pipelines and downstream systems.
CDF also powers Delta Sharing, allowing external partners to receive only the changes relevant to them rather than entire snapshots. For data warehouses handling billions of rows, CDF transforms the economics of data pipelines.
---
## Key Concepts
Enabling CDF on Tables
CDF is enabled via table properties. Once enabled, Delta tracks inserts, updates, and deletes. You can enable it at creation time or add it to existing tables. There is minimal overhead, and historical changes are available if versioning is enabled.
Reading Changes with table_changes()
The table_changes() function reads changes between two versions or timestamps. Each change record has _change_type (insert, update_preimage, update_postimage, delete) and commit_version timestamp. This enables point in time recovery and audit trails.
Incremental ETL Patterns
Instead of SELECT * FROM source WHERE updated_at > last_checkpoint, use CDF to read only changes. This works for all DML operations including updates and deletes, not just new rows. Lakeflow pipelines can consume CDF streams directly.
CDF with Delta Sharing
Delta Sharing partners can consume changes via CDF instead of receiving full table snapshots. This is crucial for large tables where sending the entire dataset per share is economically infeasible. Only changes are transmitted, reducing bandwidth and cost.
---
## Code Examples
### Enabling CDF
```sql
-- Enable CDF on new table
CREATE TABLE my_catalog.my_schema.customers (
  customer_id BIGINT,
  name STRING,
  email STRING,
  updated_at TIMESTAMP
)
USING DELTA
TBLPROPERTIES ('delta.enableChangeDataFeed' = 'true');
-- Enable on existing table
ALTER TABLE my_catalog.my_schema.customers
SET TBLPROPERTIES ('delta.enableChangeDataFeed' = 'true');
```
### Reading Changes
```sql
-- Read all changes between version 5 and 10
SELECT * FROM table_changes('my_catalog.my_schema.customers', 5, 10);
-- Read changes with filter on _change_type
SELECT customer_id, email, _change_type, commit_version
FROM table_changes('my_catalog.my_schema.customers', 0, 100)
WHERE _change_type IN ('insert', 'update_postimage');
-- Read changes since specific timestamp
SELECT * FROM table_changes(
  'my_catalog.my_schema.customers',
  timestampAsOf => '2026-02-01 00:00:00'
);
```
### Incremental ETL in Lakeflow
```python
from databricks.sdk.python.lakehouse import *
@dlt.table
def incrementally_loaded_customers():
    return (
        spark.sql("""
            SELECT *
            FROM table_changes('my_catalog.source.customers', 0, 100)
            WHERE _change_type != 'update_preimage'
        """)
    )
@dlt.table
def silver_customers():
    # Consume changes and apply business logic
    return (
        dlt.read('incrementally_loaded_customers')
        .withColumn('processed_at', current_timestamp())
    )
```
---
## Common Exam Scenarios
Scenario 1: Real Time Streaming ETL
A product catalog gets updated frequently with price and inventory changes. Instead of full scans, you enable CDF and stream changes to downstream systems in real time. Only the insert, update, and delete records are processed. This keeps multiple systems in sync with minimal latency.
Scenario 2: GDPR Compliance with Audit Trail
Customer data changes are tracked with CDF. When a deletion request arrives, you can prove which rows were deleted and when. The _change_type column shows deletes, and commit_version provides a complete audit trail for compliance demonstrations.
Scenario 3: Delta Sharing with Partners
You share customer transaction data with external partners. Sending the entire 10TB table daily is cost prohibitive. With CDF, partners receive only the daily changes via Delta Sharing, reducing bandwidth from terabytes to gigabytes.
---
## Key Takeaways
- CDF tracks inserts, updates, and deletes at the row level with _change_type and commit_version
- Enable CDF via table property for minimal overhead, available on new and existing tables
- Use table_changes() to read incremental data, reducing compute by processing only what changed
- CDF enables efficient Delta Sharing and provides audit trails for compliance
---
## Gotchas and Tips
update_preimage vs update_postimage: Updates generate two rows in CDF: preimage (old value) and postimage (new value). Filter wisely to avoid duplicates in downstream systems.
Version History Required: CDF requires Delta log retention. Very old changes may become unavailable if log retention is too short. Plan retention policies accordingly.
Exactly Once Semantics: When consuming CDF, use commit_version to track progress and avoid reprocessing the same changes.
---
## Links and Resources
- **[Change Data Feed](https://docs.databricks.com/aws/en/delta/delta-change-data-feed)** — Enabling and reading row-level change data for incremental downstream processing.
- **[Streaming with Delta Lake](https://docs.databricks.com/aws/en/structured-streaming/delta-lake)** — Using CDF with streaming queries for efficient incremental pipelines.