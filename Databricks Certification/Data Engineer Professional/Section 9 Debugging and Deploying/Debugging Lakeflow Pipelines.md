## Topic Overview
Lakeflow Spark Declarative Pipelines abstract away much of the Spark complexity, but when they fail, debugging requires understanding the pipeline execution model. Lakeflow provides an event log and expectation framework that tracks every update, every data quality check, and every failure. You can view pipeline event logs in the UI, query system tables for detailed history, and use expectation violations to identify data quality issues.
The Professional exam tests your ability to diagnose Lakeflow pipeline failures, understand the difference between development and production modes, and use event logs and lineage to troubleshoot data quality issues.
## Key Concepts
**Pipeline Event Logs for Debugging**
Lakeflow tracks every update in an event log. The log includes start time, end time, number of rows read/written, schema changes, and failures. Access the event log via the pipeline UI or by querying system.event_log (varies by workspace version). The event log shows which tables were updated, what transformations occurred, and where failures happened.
**Expectation Violations and Data Quality**
Lakeflow allows you to define expectations (data quality checks) on tables. Expectations validate conditions like no null values, row counts within range, or specific column values. When expectations fail, the pipeline can either warn or block. The event log captures expectation violations, allowing you to diagnose data quality issues without querying tables manually.
**Development vs. Production Mode Behavior**
In development mode, pipelines are more lenient: expectation failures warn but don't block the pipeline. In production mode, failed expectations can stop the pipeline. Development mode is useful for testing and incremental fixes. Production mode ensures data quality enforcement. Understanding the difference helps you debug why a pipeline succeeds in dev but fails in prod.
**Lineage Debugging for Data Tracing**
Lakeflow automatically tracks data lineage: which source tables feed into which targets, which transforms apply, and which expectations run. Use lineage to trace the path of data through the pipeline and identify which stage introduced bad data. The lineage view shows task dependencies, making it easy to understand which tables to check when a downstream table has issues.
## Code Examples
### Accessing Pipeline Event Logs
```sql
-- Query event history for a specific pipeline
SELECT timestamp, update_id, state, rows_read, rows_written, 
       error_message, expectation_violations
FROM dlt.event_log()
WHERE origin.pipeline_id = 'pipeline-12345'
ORDER BY timestamp DESC
LIMIT 50;
-- Find failed updates
SELECT timestamp, update_id, error_message
FROM dlt.event_log()
WHERE origin.pipeline_id = 'pipeline-12345'
  AND state = 'FAILED'
ORDER BY timestamp DESC;
-- Check expectation violations
SELECT timestamp, dataset, expectation, failed_records, error_message
FROM dlt.event_log()
WHERE origin.pipeline_id = 'pipeline-12345'
  AND dataset_error IS NOT NULL
ORDER BY timestamp DESC;
```
### Defining and Debugging Expectations
```python
from pyspark.sql.functions import col
import dlt
# Define a target table with expectations
@dlt.table(name="customer_silver")
@dlt.expect("valid_customer_id", "customer_id > 0")
@dlt.expect("non_null_email", "email IS NOT NULL")
@dlt.expect("valid_status", "status IN ('active', 'inactive', 'pending')")
def customer_silver():
    return (
        dlt.read("customer_bronze")
        .filter(col("customer_id").isNotNull())
        .select("customer_id", "email", "status", "created_date")
    )
# Check expectation results in the event log
# Violations are captured automatically and available in the UI
```
### Common Pipeline Failure Patterns
```sql
-- Check for schema mismatch issues
SELECT *
FROM dlt.event_log()
WHERE origin.pipeline_id = 'pipeline-id'
  AND message LIKE '%schema%'
  AND timestamp > NOW() - INTERVAL 24 HOUR;
-- Look for data validation failures
SELECT DISTINCT dataset, expectation
FROM dlt.event_log()
WHERE origin.pipeline_id = 'pipeline-id'
  AND dataset_error IS NOT NULL;
-- Identify slow stages (long update times)
SELECT update_id, timestamp,
       (unix_timestamp(end_time) - unix_timestamp(start_time)) / 60 as duration_minutes,
       rows_written
FROM dlt.event_log()
WHERE origin.pipeline_id = 'pipeline-id'
  AND state = 'COMPLETED'
ORDER BY duration_minutes DESC
LIMIT 10;
```
## Common Exam Scenarios
**A Lakeflow pipeline succeeds but downstream analysts report missing data. The event log shows rows_written = 0 for the gold table. Where would you look to diagnose?**
Check the lineage to see which silver tables feed into the gold table. Then check the event log for those silver tables: do they have rows_written > 0? If a silver table is empty, check its upstream bronze table. Look for filter conditions that might be too strict (WHERE clauses filtering out all rows). Check expectations: if an expectation failed, rows might have been rejected.
**A pipeline runs successfully in development mode but fails in production with expectation violations. You didn't change the code. What changed?**
In development mode, expectation failures are warnings and don't block the pipeline. In production mode, they stop it. The expectations are the same code; the difference is the environment setting. Check which expectation is failing in production by examining the event log. Either fix the upstream data quality, adjust the expectation threshold, or change the expectation mode to warn instead of block.
## Key Takeaways
- Event logs are your primary debugging tool for Lakeflow; check rows_written, state, and error_message first.
- Expectations validate data quality and capture violations in the event log.
- Development mode is lenient; production mode enforces strict data quality.
- Use lineage to trace which upstream tables are causing issues in downstream tables.
## Gotchas and Tips
A pipeline can appear successful (state = COMPLETED) but write zero rows due to overly strict filters. Always check rows_written in the event log, not just the pipeline state.
Expectation violations in development mode don't stop the pipeline but are still logged. Check the event log to find these failures before moving to production.
---
## Links and Resources
- **[Lakeflow Spark Declarative Pipelines](https://docs.databricks.com/aws/en/ldp/)** — Debugging pipeline failures, event logs, and data quality expectations.
- **[Lakeflow SDP Concepts](https://docs.databricks.com/aws/en/ldp/concepts)** — Understanding pipeline modes, full refresh vs incremental, and error handling.