## Topic Overview
SQL alerts let you define data quality thresholds and trigger notifications when metrics exceed them. You write a SQL query that returns a result set, then configure the alert to fire based on the query results. For example, if your query returns any rows, or if a count exceeds a threshold, the alert triggers. Notifications can go to email, Slack, PagerDuty, or webhooks. Alerts run on a schedule (hourly, daily, etc.).
SQL alerts are perfect for data quality monitoring. You can alert on null counts, distinct values dropping, late arriving data, or business logic violations. They integrate with your data platform's native SQL engine, so no additional infrastructure is needed. Combine with system tables or your own monitoring tables to build a comprehensive observability system.
## Key Concepts
**Alert Query and Trigger Condition**
The alert query returns a result set. The trigger condition determines when the alert fires. Common conditions: "Any row returned" (fires if rows exist), "Value > threshold" (fires if a numeric result exceeds a limit), "Value == threshold" (exact match). The query can reference any table in your workspace. Keep queries simple and fast since they run on a schedule.
**Notification Channels and Escalation**
Configure where alerts should be sent: email addresses, Slack channels, PagerDuty incidents, or webhooks. You can have multiple channels for a single alert. Some alerts might escalate to different channels based on severity. For critical data quality issues, send to PagerDuty. For informational alerts, email is fine.
**Alert Schedules and Frequency**
Define how often the alert query runs: every 5 minutes, hourly, daily, etc. More frequent checks catch issues faster but cost more. Less frequent checks are cheaper but might miss short lived issues. Choose frequency based on how critical the metric is and how quickly you need to respond.
**Common Alert Patterns**
Count nulls in key columns and alert if above threshold. Count distinct values and alert if they drop. Compare today's row count to yesterday and alert if significantly lower. Check for late arriving data by looking for timestamps older than expected. Validate business logic (e.g., status transitions, referential integrity).
---
## Code Examples
### Setting Up SQL Alerts for Data Quality
```sql
-- Alert: Check for high null count in key columns
-- Query: Select null counts and return rows if any exceed threshold
SELECT 
  'customer_id' as column_name,
  COUNT(*) as null_count,
  ROUND(100.0 * COUNT(*) / (SELECT COUNT(*) FROM customers), 2) as null_percentage
FROM customers
WHERE customer_id IS NULL
HAVING null_count > 100
UNION ALL
SELECT 
  'email',
  COUNT(*),
  ROUND(100.0 * COUNT(*) / (SELECT COUNT(*) FROM customers), 2)
FROM customers
WHERE email IS NULL
HAVING COUNT(*) > 100;
-- Alert: Compare row counts between today and yesterday
SELECT 
  today_count,
  yesterday_count,
  ROUND(100.0 * (yesterday_count - today_count) / yesterday_count, 2) as pct_drop
FROM (
  SELECT 
    (SELECT COUNT(*) FROM gold_sales WHERE DATE(created_at) = CURRENT_DATE) as today_count,
    (SELECT COUNT(*) FROM gold_sales WHERE DATE(created_at) = CURRENT_DATE - 1) as yesterday_count
)
WHERE today_count < yesterday_count * 0.5;  -- Alert if today is less than half of yesterday
```
### Late Data Detection Alert
```sql
-- Alert: Detect late arriving data
-- Expected: data arrives within 1 hour, processed within 24 hours
SELECT 
  source_system,
  COUNT(*) as late_record_count,
  MIN(ingestion_time) as oldest_unprocessed,
  DATEDIFF(hour, MIN(ingestion_time), CURRENT_TIMESTAMP) as hours_delayed
FROM raw_events
WHERE processed_at IS NULL
  AND ingestion_time < CURRENT_TIMESTAMP - INTERVAL 24 HOUR
GROUP BY source_system
HAVING COUNT(*) > 0;  -- Alert fires if any late records exist
```
### Business Logic Validation Alert
```sql
-- Alert: Validate referential integrity
-- Check for orders with customer_ids that don't exist in customers table
SELECT 
  COUNT(DISTINCT o.order_id) as orphaned_order_count,
  COUNT(DISTINCT o.customer_id) as affected_customers
FROM orders o
LEFT JOIN customers c ON o.customer_id = c.customer_id
WHERE c.customer_id IS NULL
HAVING COUNT(DISTINCT o.order_id) > 0;
```
---
## Common Exam Scenarios
Scenario: You want to alert your team if the daily sales volume drops more than 20% from the previous day. How would you write the alert query?
Query the gold_sales table, calculate today's and yesterday's totals, compute the percentage change, and return rows if the drop exceeds 20%. Set the trigger to "Any row returned". Send the alert to your Slack channel daily at 9am, so the team can investigate anomalies early.
Scenario: Your pipeline relies on data from a source system that sends updates hourly. You want an alert if no new data arrives within 2 hours. How do you set this up?
Write a query that checks the max timestamp in the ingest log. If it's more than 2 hours old, return a row. Set the alert to run every 15 minutes. Send critical alerts to PagerDuty so on call can respond immediately. This catches delays before downstream teams are impacted.
Scenario: You have a data quality alert that fires occasionally due to timing issues, causing alert fatigue. How would you reduce false positives?
Add a persistence check: alert only if the condition is true in 2 consecutive runs, not just once. Adjust the query to use a wider time window or more realistic thresholds. Consider the alert's frequency: if you're checking every 5 minutes, use a 15 minute window to allow for normal variance.
---
## Key Takeaways
- SQL alerts fire based on query results. If the query returns rows, or a metric exceeds a threshold, the alert triggers.
- Use alerts for data quality (nulls, duplicates, late data), business logic validation, and anomaly detection.
- Configure notification channels (email, Slack, PagerDuty, webhooks) and set alert frequency based on criticality.
- Tune alert thresholds and use persistence checks to reduce false positives and alert fatigue.
---
## Gotchas and Tips
Keep alert queries efficient. Complex queries that scan huge tables every 5 minutes are expensive. Use partitioning and indexes to speed them up.
Alert fatigue is real. If every minor variance triggers an alert, your team will ignore them. Set realistic thresholds and use domain knowledge to avoid false positives.
Document what each alert measures and why. Include remediation steps in the alert message so responders know what to do.
---
## Links and Resources
- **[SQL Alerts](https://docs.databricks.com/aws/en/sql/user/alerts/)** — Setting up threshold-based alerts on SQL query results for data quality monitoring.