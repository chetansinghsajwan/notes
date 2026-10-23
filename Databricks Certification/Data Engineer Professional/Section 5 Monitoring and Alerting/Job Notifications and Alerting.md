## Topic Overview
Databricks jobs have built in alerting capabilities. You can configure email notifications when a job succeeds, fails, or times out. Webhooks allow you to send job completion signals to external systems like Slack, PagerDuty, or your own monitoring platform. Job notifications are triggered automatically based on job state changes, so your team stays informed without manual polling.
For real time streaming jobs, Databricks provides health rules that monitor query progress and trigger alerts if a streaming table stalls or fails. System tables also record job execution metrics that you can query to build custom alerting on top. Together, these tools let you build comprehensive job health monitoring for your production data pipelines.
## Key Concepts
**Email Notifications on Job State Changes**
Configure notifications at job creation or update time. Specify email addresses and which events trigger notifications: on_success, on_failure, on_timeout. The email includes the job name, run ID, status, and a link to the run details. Multiple email addresses can be configured for a single job.
**Webhook Integrations**
Webhooks send HTTP POST requests to external systems when job events occur. Configure a webhook URL and which events to notify on. Databricks sends a JSON payload with job details. Use webhooks to trigger notifications in Slack, create PagerDuty incidents, or integrate with custom monitoring systems. Webhooks are more flexible than email for automation.
**Health Rules for Streaming Jobs**
Streaming tables have health rules that automatically detect stalled or failed queries. These rules monitor query lag, error rates, and data freshness. When a rule is violated, the job is marked as unhealthy and notifications can be sent. Health rules prevent silent failures in streaming pipelines.
**Job Run Metrics from System Tables**
Query system.jobs and system.job_runs tables to get historical job execution data. Metrics include start time, end time, status, error message, and compute resources used. Use these tables to build custom alerting on job SLAs, failure patterns, or performance degradation.
---
## Code Examples
### Configuring Job Notifications (SQL/API)
```python
# Using Databricks Python SDK to create a job with notifications
from databricks.sdk import WorkspaceClient
client = WorkspaceClient()
job = client.jobs.create(
    name="daily_sales_pipeline",
    tasks=[
        {
            "task_key": "process_sales",
            "notebook_task": {
                "notebook_path": "/Workspace/Users/me/process_sales"
            },
            "cluster_id": "cluster-123"
        }
    ],
    email_notifications={
        "on_success": ["team@company.com"],
        "on_failure": ["oncall@company.com"]
    },
    webhook_notifications={
        "on_failure": [{
            "id": "slack-webhook"
        }],
        "on_success": [{
            "id": "pagerduty-webhook"
        }]
    }
)
```
### Querying Job Run Metrics
```sql
-- Query job run metrics from system tables
SELECT 
  job_id,
  run_id,
  state,
  start_time,
  end_time,
  DATEDIFF(minute, start_time, end_time) as duration_minutes,
  state_message
FROM system.job_runs
WHERE start_time >= CURRENT_TIMESTAMP - INTERVAL 7 DAY
  AND state IN ('FAILED', 'TIMEOUT')
ORDER BY start_time DESC;
-- Alert on jobs with high failure rate
SELECT 
  job_id,
  COUNT(CASE WHEN state = 'FAILED' THEN 1 END) as failures,
  COUNT(*) as total_runs,
  ROUND(100.0 * COUNT(CASE WHEN state = 'FAILED' THEN 1 END) / COUNT(*), 2) as failure_rate
FROM system.job_runs
WHERE start_time >= DATE_SUB(CURRENT_TIMESTAMP, 1)
GROUP BY job_id
HAVING failure_rate > 10
ORDER BY failure_rate DESC;
```
### Webhook Payload Example
```json
{
  "job_id": 123,
  "run_id": 456,
  "job_name": "daily_sales_pipeline",
  "state": "FAILED",
  "error_message": "Task failed: Invalid data format in source file",
  "start_time": "2024-01-15T09:00:00Z",
  "end_time": "2024-01-15T09:15:30Z",
  "duration_seconds": 930,
  "owner": "data-team@company.com",
  "run_url": "https://workspace.cloud.databricks.com/?o=org-id\#job/123/run/456"
}
```
---
## Common Exam Scenarios
Scenario: Your team wants immediate notification when a critical job fails. Email is too slow. What's the best approach?
Configure a webhook on the job that posts to a Slack channel when on_failure occurs. Add a second webhook to PagerDuty to create an incident for on call. This way, the team sees the failure immediately in Slack and on call gets an incident to investigate.
Scenario: You need to track SLAs for job completion. Jobs should finish within 1 hour. How would you detect and alert on violations?
Query system.job_runs to find runs where `DATEDIFF(minute, start_time, end_time) > 60`. Create a SQL alert that returns rows if any violations occur. Set the alert to run daily and send notifications to your team. Track SLA compliance over time by storing violations in a metrics table.
Scenario: A streaming pipeline is supposed to update real time, but data freshness has degraded. You want to alert if data is older than 15 minutes. What health rule would you configure?
Configure a health rule on the streaming table's update latency. If the table hasn't been updated in 15 minutes, the rule fails and marks the streaming table as unhealthy. Pair this with job notifications so your team is alerted immediately when freshness degrades.
---
## Key Takeaways
- Job notifications include email and webhooks. Configure them on job creation or update to alert on success, failure, or timeout events.
- Webhooks are more flexible and enable integrations with Slack, PagerDuty, and custom systems. Use them for critical jobs that need immediate response.
- Health rules for streaming jobs automatically detect stalled or failed queries and enable proactive monitoring.
- Query system.job_runs to build custom alerting on job SLAs, failure patterns, and performance metrics.
---
## Gotchas and Tips
Webhooks need to be public and reachable from Databricks. Test the webhook URL before configuring it on the job. If the webhook fails, Databricks may retry, which could trigger duplicate notifications.
Email notifications can get lost or filtered. For critical jobs, always use webhooks for immediate visibility. Email is fine for informational notifications.
Webhook payloads have a maximum size. If your payload is huge, consider sending only essential fields or storing full details in system tables for later query.
---
## Links and Resources
- **[Job Notifications](https://docs.databricks.com/aws/en/jobs/notifications)** — Configuring email, webhook, and PagerDuty notifications for job events.
- **[Lakeflow Jobs](https://docs.databricks.com/aws/en/jobs/)** — Overview of job orchestration and alerting capabilities.