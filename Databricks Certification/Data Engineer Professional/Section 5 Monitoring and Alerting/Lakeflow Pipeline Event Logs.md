## Topic Overview
Lakeflow Spark Declarative Pipelines produce event logs that track pipeline execution, data quality expectations, and lineage. These logs are stored in a Delta table and queryable with SQL. You can use them to understand pipeline health, debug failures, and validate that data quality rules passed. The event log is part of the pipeline's operational visibility, complementing the Databricks UI dashboard.
The event log includes records for when tables start updating, expectations (data quality checks) pass or fail, and when pipelines complete. Each record has a timestamp, dataset name, action type, and details. By querying the event log, you can build historical reports on pipeline performance and data quality trends.
## Key Concepts
**Event Log Table Structure**
The event log is a Delta table in the pipeline's metadata directory. Columns include timestamp, dataset (table name), action (START_UPDATE, END_UPDATE, EXPECTATION_PASSED, EXPECTATION_FAILED), details (JSON with specifics), and others. Each row represents an event in the pipeline lifecycle. The table is managed automatically by Databricks.
**Pipeline Expectations and Data Quality**
Expectations are data quality rules defined in your pipeline code. They run on each table update. Results (EXPECTATION_PASSED or EXPECTATION_FAILED) are logged with the failing rows or metrics. By querying the event log for EXPECTATION_FAILED records, you can track data quality issues and alert when thresholds are breached.
**Querying Event Logs for Monitoring**
Query the event log table with standard SQL filters. Find pipeline runs by date, check which tables updated, and see if expectations passed. Join with your own tables to correlate pipeline events with business metrics. The event log is a complete audit trail of pipeline behavior.
**Update History and Lineage**
The event log tracks which tables were updated in each pipeline run. By analyzing START_UPDATE and END_UPDATE events, you can build a lineage of data flow through your pipeline. This helps understand dependencies and detect if critical tables failed to update.
---
## Code Examples
### Querying Pipeline Event Logs
```sql
-- Query the event log for a pipeline
-- Assuming event log is at: "/Pipelines/pipeline_id/event_log"
SELECT 
  timestamp,
  dataset,
  action,
  details
FROM pipeline_event_log
WHERE timestamp >= CURRENT_DATE - 7
ORDER BY timestamp DESC;
-- Find pipeline runs by looking for START_UPDATE events
SELECT 
  DATE(timestamp) as run_date,
  COUNT(DISTINCT DATE_TRUNC('hour', timestamp)) as updates
FROM pipeline_event_log
WHERE action = 'START_UPDATE'
GROUP BY DATE(timestamp)
ORDER BY run_date DESC;
-- Track data quality failures
SELECT 
  dataset,
  COUNT(*) as failure_count,
  MAX(timestamp) as last_failure
FROM pipeline_event_log
WHERE action = 'EXPECTATION_FAILED'
GROUP BY dataset
ORDER BY failure_count DESC;
```
### Monitoring Table Update Success Rates
```sql
-- Track table update success rates
SELECT 
  dataset,
  COUNT(CASE WHEN action = 'END_UPDATE' THEN 1 END) as successful_updates,
  COUNT(CASE WHEN action = 'UPDATE_FAILED' THEN 1 END) as failed_updates,
  ROUND(100.0 * COUNT(CASE WHEN action = 'END_UPDATE' THEN 1 END) / 
    (COUNT(CASE WHEN action = 'START_UPDATE' THEN 1 END)), 2) as success_rate
FROM pipeline_event_log
WHERE timestamp >= DATE_SUB(CURRENT_DATE, 30)
GROUP BY dataset
ORDER BY success_rate ASC;
-- Identify slow table updates
SELECT 
  dataset,
  MIN(CASE WHEN action = 'START_UPDATE' THEN timestamp END) as start_time,
  MAX(CASE WHEN action = 'END_UPDATE' THEN timestamp END) as end_time,
  DATEDIFF(minute, 
    MIN(CASE WHEN action = 'START_UPDATE' THEN timestamp END),
    MAX(CASE WHEN action = 'END_UPDATE' THEN timestamp END)
  ) as duration_minutes
FROM pipeline_event_log
GROUP BY dataset
HAVING duration_minutes > 60
ORDER BY duration_minutes DESC;
```
### Extracting Expectation Details from Event Logs
```sql
-- Extract details from expectation failures
SELECT 
  timestamp,
  dataset,
  details:expectation_name as expectation_name,
  details:expectation_type as expectation_type,
  details:failed_rows as failed_rows
FROM pipeline_event_log
WHERE action = 'EXPECTATION_FAILED'
  AND timestamp >= CURRENT_DATE - 1
ORDER BY timestamp DESC;
-- Summary of expectations by table
SELECT 
  dataset,
  details:expectation_name as expectation_name,
  COUNT(*) as failure_count,
  SUM(CAST(details:failed_rows as int)) as total_failed_rows
FROM pipeline_event_log
WHERE action = 'EXPECTATION_FAILED'
  AND timestamp >= DATE_SUB(CURRENT_DATE, 7)
GROUP BY dataset, expectation_name
ORDER BY failure_count DESC;
```
---
## Common Exam Scenarios
Scenario: A Lakeflow pipeline failed silently (no error in the UI), but downstream tables weren't updated. How would you investigate?
Query the pipeline's event log for START_UPDATE events on the downstream tables. If you see START_UPDATE but no END_UPDATE for a table, the update failed or hung. Look for UPDATE_FAILED or EXPECTATION_FAILED events for that table. The event log details will show the error message.
Scenario: You want to alert when a critical data quality expectation fails 3 times in 24 hours. How would you set this up?
Query the event log for EXPECTATION_FAILED on that specific table and expectation. Count failures in the last 24 hours. Use a SQL alert that triggers when the count exceeds 2. Configure the alert to send notifications to your team (email, Slack, etc.).
Scenario: You're debugging why table updates are taking longer than usual. What event log information would you examine?
Look at the time difference between START_UPDATE and END_UPDATE for each table over the past week. Compare to historical averages. If a specific table's duration increased, check for EXPECTATION_FAILED or other anomalies in the details column. The event log tells you exactly when updates started and stopped, revealing performance regressions.
---
## Key Takeaways
- Lakeflow pipeline event logs are Delta tables containing pipeline execution events. Query them with SQL to monitor pipeline health and data quality.
- Events include START_UPDATE, END_UPDATE, EXPECTATION_PASSED, EXPECTATION_FAILED, and others. Each event has a timestamp and detailed metadata.
- Track data quality by querying EXPECTATION_FAILED events. Extract failure details from the event JSON to understand which rules failed and why.
- Use event log timestamps to build lineage and detect if critical tables failed to update. Alert on patterns like repeated failures.
---
## Gotchas and Tips
Event log retention is finite. Databricks may archive old event logs. For long term compliance auditing, copy important events to a separate table that you manage.
The details column is JSON. When extracting values, use the correct syntax for your SQL engine (e.g., `details:field_name` in Databricks SQL). Test your queries to ensure you're parsing JSON correctly.
Event log timestamps are important. Use them for debugging, but remember there may be slight delays between event generation and log write. Don't rely on sub second precision.
---
## Links and Resources
- **[Lakeflow Spark Declarative Pipelines](https://docs.databricks.com/aws/en/ldp/)** — Monitoring pipeline health and reviewing event logs for debugging.
- **[Lakeflow SDP Concepts](https://docs.databricks.com/aws/en/ldp/concepts)** — Understanding pipeline event log schema and metrics.