## Topic Overview
Databricks **audit logs** track all user activity and data access events within your workspace. Every action a user takes, from logging in to reading or writing data, gets recorded. These logs are stored in system tables and provide a complete audit trail for compliance, security monitoring, and troubleshooting.
The primary system table for audit data is `system.access.audit` which contains detailed information about who accessed what, when they accessed it, and from where. This becomes critical when you need to demonstrate compliance with data governance policies, investigate unauthorized access attempts, or understand data lineage and usage patterns.
On the exam, you'll encounter questions about querying audit logs, understanding what events are tracked, configuring audit log delivery, and using audit data for compliance monitoring. Knowing how to extract meaningful insights from audit logs using SQL is a key skill for data engineers working with Unity Catalog and governed environments.
---
## Key Concepts
What Audit Logs Capture
Audit logs capture virtually every action in your Databricks workspace. This includes user authentication events (logins, logouts), cluster operations (creation, deletion, resizing), workspace object access (reading or modifying notebooks, dashboards), table and view access, permission grants and revokes, and Unity Catalog operations. Each event records the timestamp, the identity of the user or service principal who performed the action, the action name, request parameters, response status, and metadata about the request source (IP address, user agent).
system.access.audit Table Structure
The `system.access.audit` table is a system managed table available in every Databricks workspace. Key columns include: `event_time` (timestamp of the event), `event_id` (unique identifier), `workspace_id` (workspace identifier), `identity` (user principal or service principal), `action_name` (the action performed), `request_params` (JSON containing request details), and `response` (HTTP status code and error messages if applicable).
Common Audit Events
Events commonly tracked and tested on the exam include: `login` (user authentication), `logout`, `createCluster`, `deleteCluster`, `readTable`, `writeTable`, `grantPermission`, `revokePermission`, and `modifyPermission`. Unity Catalog specific events track `createTable`, `deleteTable`, `updateTableTag`, and more.
Querying Audit Data
You query audit logs like any other table using SQL or PySpark. Since `system.access.audit` is a system table, you must have appropriate permissions to query it. The `request_params` column often contains nested JSON, so you'll use JSON functions like `get_json_object` or `from_json` to extract nested fields. Filtering by `event_time` is crucial for performance, especially on large audit logs that span weeks or months.
Retention and Delivery
Audit logs are retained for 90 days by default in the system table. For longer term retention and compliance, Databricks allows you to configure audit log delivery to cloud storage (S3, Azure Blob Storage, GCS) where they are archived in Parquet format. This is done through workspace settings. Once delivered to cloud storage, you can integrate with SIEM tools (Splunk, Datadog, Sentinel) for centralized monitoring and alerting. Some organizations use this capability to meet regulatory requirements like GDPR, HIPAA, and SOC 2.
---
## Code Examples
Query audit logs for table access events:
```sql
SELECT
  event_time,
  identity,
  action_name,
  request_params,
  response
FROM system.access.audit
WHERE action_name IN ('readTable', 'writeTable')
  AND event_time >= CURRENT_DATE() - INTERVAL 7 DAYS
ORDER BY event_time DESC;
```
Find who accessed a specific table:
```sql
SELECT
  DISTINCT identity,
  action_name,
  COUNT(*) as access_count,
  MIN(event_time) as first_access,
  MAX(event_time) as last_access
FROM system.access.audit
WHERE request_params LIKE '%my_table%'
  AND event_time >= CURRENT_DATE() - INTERVAL 30 DAYS
GROUP BY identity, action_name
ORDER BY access_count DESC;
```
Track permission changes (grants and revokes):
```sql
SELECT
  event_time,
  identity,
  action_name,
  get_json_object(request_params, '$.principal_name') as principal_name,
  get_json_object(request_params, '$.permission') as permission,
  response
FROM system.access.audit
WHERE action_name IN ('grantPermission', 'revokePermission')
  AND event_time >= CURRENT_DATE() - INTERVAL 14 DAYS
ORDER BY event_time DESC;
```
Monitor failed login attempts:
```sql
SELECT
  event_time,
  identity,
  get_json_object(request_params, '$.remote_address') as ip_address,
  COUNT(*) as failed_attempts
FROM system.access.audit
WHERE action_name = 'login'
  AND response LIKE '%401%'
  AND event_time >= CURRENT_DATE() - INTERVAL 1 DAYS
GROUP BY event_time, identity, ip_address
HAVING COUNT(*) > 3
ORDER BY failed_attempts DESC;
```
---
## Common Exam Scenarios
**Scenario 1: Compliance Audit** Your organization needs to prove that only authorized users accessed sensitive customer data in the past 30 days. You need to query `system.access.audit` for all table access events, filter by `action_name IN ('readTable', 'writeTable')` and a specific table name in the `request_params` column, and group by user to show access patterns. The exam tests your ability to filter correctly by date range (CURRENT_DATE() - INTERVAL) and understand what fields contain the information you need.
**Scenario 2: Security Investigation** You suspect unauthorized access attempts. You need to find failed login attempts by querying audit logs where `action_name = 'login'` and the `response` field contains an error code (e.g., 401, 403). The test asks you to identify which column shows failed status and how to extract the IP address from `request_params` using JSON functions like `get_json_object`.
**Scenario 3: Permission Change Tracking** Your organization needs to audit who granted permissions and to whom. You query audit logs for `action_name IN ('grantPermission', 'revokePermission')` and extract principal names and permission types from the nested JSON in `request_params`. The exam tests whether you know that permission events are tracked and how to extract nested data from request parameters.
---
## Key Takeaways
- Audit logs are stored in the `system.access.audit` system table and track every action in your workspace, including logins, cluster operations, and data access events.
- Key columns include `event_time`, `identity`, `action_name`, `request_params` (JSON), and `response` (status codes). Use `get_json_object` to extract nested fields from request_params.
- Audit logs are retained for 90 days in the system table by default. For longer term retention, configure audit log delivery to cloud storage in workspace settings.
- Common examined actions include `login`, `readTable`, `writeTable`, `grantPermission`, and `revokePermission`. Always filter queries by event_time for performance on large datasets.
- Audit logs can be delivered to SIEM tools (Splunk, Datadog, Sentinel) for centralized monitoring and are critical for demonstrating compliance with regulations like GDPR and HIPAA.
---
## Gotchas and Tips
Only 90 day retention by default. If an audit log event happened more than 90 days ago, it will not be in `system.access.audit`. Always configure audit log delivery to cloud storage immediately if you need longer term compliance records. This is a common test point.
request_params contains nested JSON. Use `get_json_object` or parse the JSON properly. Simply using LIKE patterns (e.g., LIKE '%table_name%') works for quick searches but is not reliable for structured queries. Know how to extract nested fields for the exam.
Always filter by `event_time` in your queries. Scanning months of audit logs without a date filter will be slow. The exam may test your understanding of query performance on large system tables.
---
## Links and Resources
- **[Audit Log System Tables](https://docs.databricks.com/aws/en/admin/system-tables/audit-logs)** — Reference for audit log system table schema and querying audit events.
- **[System Tables Overview](https://docs.databricks.com/aws/en/admin/system-tables/)** — Overview of all available system tables for monitoring and observability.