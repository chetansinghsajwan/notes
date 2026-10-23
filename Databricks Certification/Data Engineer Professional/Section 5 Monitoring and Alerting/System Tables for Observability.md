## Topic Overview
Databricks system tables give you direct access to metadata about billing, compute, access, and query execution. These tables are in the `system` schema and contain real time information about your workspace. You can query them with SQL just like regular tables, which makes it easy to build dashboards, alerts, and compliance reports. System tables are the foundation for observability in a modern data lakehouse.
The main system tables are `system.billing.usage` for cost tracking, `system.compute.*` tables for cluster and job metrics, `system.access` for audit logs, and `system.query_history` for SQL and DataFrame execution details. These tables update continuously as activity happens in your workspace. You can join them with your own tables to correlate costs with business outcomes, track resource consumption by team, or detect anomalies in data access patterns.
## Key Concepts
**System Tables and Schemas**
System tables live in the `system` catalog and are organized into schemas like `system.billing`, `system.compute`, `system.access`, and `system.query`. Each schema contains tables related to a specific aspect of workspace operations. You access them with three part naming: `system.schema.table`. System tables are read only. They're managed by Databricks and updated automatically.
**Billing and Cost Tracking**
The `system.billing.usage` table tracks compute cost, storage cost, and other charges. Each row represents usage for a workspace, cluster, or job during a time interval. You can query it to find which clusters or jobs are most expensive, track trends over time, or allocate costs to departments. The table includes fields for usage quantity, rate, and cost.
**Access Logs and Audit Trail**
The `system.access` table records every action that requires Unity Catalog permission checks. This includes table reads, writes, and metadata operations. Each row has timestamp, user identity, action type (SELECT, INSERT, etc.), and the object accessed. It's the authoritative source for compliance and auditing. Filter by action type and timestamp to find specific events.
**Query Execution History**
The `system.query_history` table stores details about every SQL query and DataFrame operation executed in the workspace. Fields include query text, execution duration, rows scanned, user identity, and compute resources used. This is useful for identifying slow queries, finding expensive operations, and understanding usage patterns.
---
## Code Examples
### Cost Analysis Using Billing Tables
```sql
-- Find the most expensive clusters
SELECT 
  cluster_id,
  SUM(cost) as total_cost,
  COUNT(*) as usage_records
FROM system.billing.usage
WHERE usage_date >= DATE_SUB(CURRENT_DATE, 30)
  AND cluster_id IS NOT NULL
GROUP BY cluster_id
ORDER BY total_cost DESC;
-- Track daily compute costs by workspace
SELECT 
  usage_date,
  workspace_id,
  SUM(cost) as daily_cost
FROM system.billing.usage
WHERE usage_date >= DATE_SUB(CURRENT_DATE, 7)
  AND sku_name = 'COMPUTE'
GROUP BY usage_date, workspace_id
ORDER BY usage_date DESC, daily_cost DESC;
```
### Access Audit and Compliance
```sql
-- Find all data access in the last 7 days
SELECT 
  timestamp,
  user_identity.user_name,
  action,
  request.full_name
FROM system.access
WHERE timestamp >= DATE_SUB(CURRENT_TIMESTAMP, 7)
  AND action IN ('SELECT', 'INSERT', 'UPDATE', 'DELETE')
ORDER BY timestamp DESC;
-- Track who accessed sensitive tables
SELECT 
  user_identity.user_name,
  COUNT(*) as access_count,
  MAX(timestamp) as last_access
FROM system.access
WHERE request.full_name LIKE '%.gold_customers' 
  AND action = 'SELECT'
GROUP BY user_identity.user_name
ORDER BY access_count DESC;
```
### Query Performance Analysis
```sql
-- Find the slowest queries
SELECT 
  query_id,
  statement_type,
  total_user_time_ms / 1000.0 as duration_seconds,
  rows_produced,
  user_name
FROM system.query_history
WHERE start_time >= CURRENT_TIMESTAMP - INTERVAL 24 HOUR
ORDER BY total_user_time_ms DESC
LIMIT 10;
-- Track query execution by user
SELECT 
  user_name,
  COUNT(*) as query_count,
  AVG(total_user_time_ms) as avg_duration_ms,
  SUM(rows_produced) as total_rows_produced
FROM system.query_history
WHERE start_time >= DATE_SUB(CURRENT_TIMESTAMP, 7)
GROUP BY user_name
HAVING query_count > 10
ORDER BY query_count DESC;
```
---
## Common Exam Scenarios
Scenario: Your manager asks which table in the data lake is being accessed most frequently. How do you find this?
Query the `system.access` table filtered by action = 'SELECT' and the table name. Group by the object name and count the access events. You can filter by time range to see trends. This tells you which tables are most valuable and should be optimized or monitored closely.
Scenario: You're building a cost allocation model for your data engineering team. How do you use system tables to track costs by team or project?
Query `system.billing.usage` and join it with your own metadata tables that map cluster IDs or job names to teams or projects. Sum costs by team and time period. You can also tag clusters and jobs with metadata labels, then parse those in the billing table. This gives you a way to charge back costs to the right teams.
Scenario: An external auditor asks for a report of all data modifications in the past 90 days, showing who changed what and when.
Query `system.access` filtered for actions INSERT, UPDATE, DELETE with a timestamp >= 90 days ago. Include user identity, object name, action type, and timestamp. Export to CSV or generate a dashboard. This is a compliance standard requirement and system.access is the source of truth.
---
## Key Takeaways
- System tables provide real time data about workspace operations, costs, access, and query execution. Query them with SQL to build observability into your data platform.
- `system.billing.usage` tracks costs by cluster, job, and workspace. Use it for cost allocation, budgeting, and identifying expensive operations.
- `system.access` is the audit trail for data access. Query it for compliance, security monitoring, and tracking data lineage by user.
- `system.query_history` records query execution details. Use it to find slow queries, identify expensive operations, and understand usage patterns.
---
## Gotchas and Tips
System tables have retention periods. Old records are automatically purged to manage storage. For compliance, export important audit or billing data to your own tables regularly if you need longer term retention.
Querying system tables counts as a query and uses compute. If you run large or frequent queries on system tables, monitor the cost. Consider materializing important views or exporting summaries periodically rather than querying raw system tables every time.
System tables are read only and cannot be modified directly. You can create views or tables based on system table data, but the original system tables themselves are immutable.
---
## Links and Resources
- **[System Tables Overview](https://docs.databricks.com/aws/en/admin/system-tables/)** — Querying system tables for billing, audit, compute, and lineage observability.
- **[Audit Log System Tables](https://docs.databricks.com/aws/en/admin/system-tables/audit-logs)** — Monitoring workspace activity and user actions via audit logs.