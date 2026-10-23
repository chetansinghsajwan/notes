## Topic Overview
Change Data Capture (CDC) is a pattern for capturing and propagating data changes from a source system into your Lakehouse. In Lakeflow Spark Declarative Pipelines, the APPLY CHANGES API simplifies CDC implementation by automatically handling Slowly Changing Dimensions (SCD) Type 1 and Type 2 logic. Instead of writing complex SQL merge statements yourself, you declare the source of changes, the keys that identify records, and how to apply them. Lakeflow manages the rest: sequencing changes in the correct order, handling duplicates, and maintaining historical records if needed.
The APPLY CHANGES syntax is designed to work seamlessly with streaming sources like Kafka, Kinesis, or cloud storage (when polled). You specify the target table where changes land, the keys that identify rows, and optionally the sequencing field that determines the order of changes. For SCD Type 2, Lakeflow automatically manages start_date and end_date columns and closure logic without you having to code it.
On the exam, you will encounter scenarios asking you to choose between SCD Type 1 (overwrite) vs Type 2 (keep history), how to sequence changes correctly from a streaming source, and how to apply transformations alongside CDC logic. Understanding the trade offs between these approaches and when to use each is critical.
---
## Key Concepts
**SCD Type 1 vs SCD Type 2**
SCD Type 1 (Slowly Changing Dimension Type 1) overwrites the entire row when a change is detected. No history is kept; only the current state exists. Use Type 1 when you do not need to track changes over time, such as correcting a misspelled name or updating a customer's current address.
SCD Type 2 (Slowly Changing Dimension Type 2) creates a new row for each change and marks the previous version as closed. It maintains a complete audit trail. Lakeflow automatically manages start_date and end_date columns so you can easily query the state of a record at any point in time. Use Type 2 when you need historical context, like tracking how a product's price changed over time.
**Keys and Sequence By**
The keys parameter identifies which columns uniquely identify a row (like a primary key). Lakeflow uses keys to match incoming changes to existing records.
The sequence_by parameter specifies the column that determines the order in which changes are applied (usually a timestamp or sequence number). This is critical when changes arrive out of order from a streaming source. Without proper sequencing, you might apply an old change after a newer one, corrupting your data.
**Applying Changes from Streaming Sources**
Lakeflow's APPLY CHANGES is designed to work with streaming tables fed from sources like Kafka. Changes flow in continuously, and Lakeflow ensures they are applied in order, even if micro batches arrive out of sequence. The declarative nature means you don't worry about managing state or checkpoints yourself.
**Where Clause and Filtering**
APPLY CHANGES supports an optional WHERE clause to filter which rows from the source are processed. For example, you might only apply changes where status is not 'deleted', or only include records from a specific region. This keeps your target table clean and focused.
---
## Code Examples
### SCD Type 1: APPLY CHANGES SQL
```sql
APPLY CHANGES INTO my_catalog.my_schema.customers
FROM stream customer_changes
KEYS (customer_id)
SEQUENCE BY updated_at
WHEN MATCHED THEN UPDATE SET *
WHEN NOT MATCHED THEN INSERT *
```
This Type 1 SCD applies each change directly, overwriting the entire row. Old values are lost. Suitable for corrections or simple updates where history is not needed.
### SCD Type 2: APPLY CHANGES SQL with History
```sql
APPLY CHANGES INTO my_catalog.my_schema.products
FROM stream product_changes
KEYS (product_id)
SEQUENCE BY change_timestamp
WHEN MATCHED AND (change_type = 'UPDATE') THEN UPDATE SET *
WHEN MATCHED AND (change_type = 'DELETE') THEN DELETE
WHEN NOT MATCHED THEN INSERT *;
```
For Type 2, Lakeflow automatically adds start_date and end_date columns. When a row is updated, the old version is closed (end_date set) and a new version is inserted. This maintains full audit trail for temporal queries.
### Python dlt.apply_changes (Lakeflow Pipelines)
```python
import dlt
from pyspark.sql import functions as F
@dlt.table
def customer_raw():
    return spark.readStream.format("cloudFiles") \
        .option("cloudFiles.format", "json") \
        .option("cloudFiles.schemaLocation", "/checkpoints/customers") \
        .load("/source/customers")
@dlt.table
def customers_scd2():
    return dlt.apply_changes(
        target="my_catalog.my_schema.customers_scd2",
        source="customer_raw",
        keys=["customer_id"],
        sequence_by="updated_at",
        as_of_delta_timestamp="2024-01-01",
        ignore_null_updates=False,
        apply_as_merge=True
    )
```
The Python API handles CDC declaratively. You define the streaming source, specify keys and sequencing, and Lakeflow manages the SCD logic for you. The ignore_null_updates flag lets you skip updates where certain columns are null (useful when partial updates arrive from the source).
---
## Common Exam Scenarios
**Scenario 1: Customer address history tracking**
Your retail company receives customer address updates from an upstream system. Some customers have moved multiple times, and you need to be able to generate reports like 'which states did customer X live in during 2023?' and 'what was the customer's primary address on January 15, 2024?' You would choose SCD Type 2 to maintain the full history with start_date and end_date. APPLY CHANGES automatically manages these temporal columns, so your downstream analysts can simply filter the table where start_date <= query_date AND (end_date IS NULL OR end_date > query_date).
**Scenario 2: Out of order change events**
Data arrives from Kafka in micro batches, but network delays mean that changes for product ID 1234 arrive in the wrong order: a price update from 10:00 AM arrives after a price update from 9:00 AM. Without the sequence_by parameter, you would overwrite the 9 AM price with the 10 AM price even though it happened first. By specifying sequence_by with a reliable timestamp, Lakeflow ensures changes are applied in logical order regardless of physical arrival sequence.
**Scenario 3: Filtering invalid changes**
Your source stream includes test records and deletions mixed with production updates. You add a WHERE clause to APPLY CHANGES to filter out rows where environment != 'production' or where change_type = 'TEST'. This keeps your target table clean without requiring a separate transformation step before CDC.
---
## Key Takeaways
- APPLY CHANGES simplifies CDC by handling SCD Type 1 (overwrite) and Type 2 (keep history) without manual merge code
- Use keys to identify matching rows and sequence_by to ensure changes are applied in logical order, even if micro batches arrive out of sequence
- Lakeflow automatically manages start_date and end_date columns for Type 2, enabling temporal queries without additional code
- Filter source changes with WHERE clauses to exclude test data or invalid records before they land in the target table
- APPLY CHANGES works seamlessly with streaming sources like Kafka and cloud storage, making it ideal for real time CDC pipelines
---
## Gotchas and Tips
⚠️ Sequence by must be reliable: If your sequence_by column has duplicate timestamps or is not monotonically increasing, Lakeflow may not apply changes in the correct order. Use high resolution timestamps or sequence numbers from the source system.
⚠️ Type 1 vs Type 2 trade off: SCD Type 1 is faster and uses less storage, but you lose historical context. Only use Type 1 when you truly do not need to audit changes over time.
⚠️ Keys must be stable: If your key values change (e.g., customer_id gets reassigned), APPLY CHANGES treats it as a new row. Ensure keys are immutable identifiers.
---
## Links and Resources
- **[APPLY CHANGES APIs for CDC](https://docs.databricks.com/aws/en/ldp/cdc)** — Processing change data capture feeds with APPLY CHANGES and APPLY CHANGES FROM SNAPSHOT.
- **[Change Data Feed](https://docs.databricks.com/aws/en/delta/delta-change-data-feed)** — Reading row-level changes from Delta tables for downstream processing.