## Topic Overview
The Databricks REST APIs and CLI give you programmatic control over your workspace. You can start jobs, check their status, manage clusters, query pipeline progress, and retrieve results without using the UI. This is essential for production workflows where you need to monitor job health, trigger data pipelines from external systems, or build custom observability dashboards.
The main APIs you'll use are the Jobs API to create and monitor jobs, the Clusters API to manage cluster lifecycle, and the Pipelines API to monitor Lakeflow Spark Declarative Pipelines. Each API has a REST endpoint and a CLI equivalent. Common patterns include polling for job completion, setting up webhooks for notifications, and extracting metadata for compliance audits.
## Key Concepts
**Jobs API Endpoints**
The Jobs API lets you create, update, list, and delete jobs, as well as monitor runs. Key endpoints include `POST /api/2.1/jobs/create` to create a job, `POST /api/2.1/jobs/runs/submit` to trigger a run, and `GET /api/2.1/jobs/runs/get` to retrieve run details. Each request requires authentication via a personal access token or service principal.
**Polling and Webhooks**
Polling means repeatedly calling the API to check job status. You poll the runs/get endpoint every few seconds until the job completes. Webhooks are more efficient: the API notifies you when a job succeeds or fails by sending an HTTP POST to your callback URL. Configure webhooks when creating or updating a job to get real time notifications.
**CLI Equivalent Operations**
The Databricks CLI mirrors the REST API. For every REST endpoint, there's a corresponding CLI command. For example, `databricks jobs create` is equivalent to `POST /api/2.1/jobs/create`. The CLI is convenient for scripting and running commands from the shell. You authenticate with a personal access token in your `.databrickscfg` file.
**Monitoring Job and Cluster Status**
Use the runs/list endpoint to get all runs for a job, filtering by state (RUNNING, SUCCEEDED, FAILED). Combine with query parameters to find runs within a time range. The response includes run ID, state, start/end time, and result details. For clusters, use the Clusters API to check state (RUNNING, PENDING, TERMINATED) and collect metrics.
---
## Code Examples
### Using the Jobs API via REST
```bash
# Authenticate
token="dapi1234567890abcdef"
workspace_url="https://myworkspace.cloud.databricks.com"
# Submit a job run
curl -X POST \
  "${workspace_url}/api/2.1/jobs/runs/submit" \
  -H "Authorization: Bearer ${token}" \
  -H "Content-Type: application/json" \
  -d '{
    "job_id": 123,
    "notebook_params": {
      "date": "2024-01-01"
    }
  }'
# Get run status
run_id="456"
curl -X GET \
  "${workspace_url}/api/2.1/jobs/runs/get?run_id=${run_id}" \
  -H "Authorization: Bearer ${token}"
# List recent runs for a job
job_id="123"
curl -X GET \
  "${workspace_url}/api/2.1/jobs/runs/list?job_id=${job_id}&limit=10" \
  -H "Authorization: Bearer ${token}"
```
### Using the Databricks CLI
```bash
# Set up authentication in ~/.databrickscfg
# [DEFAULT]
# host = https://myworkspace.cloud.databricks.com
# token = dapi1234567890abcdef
# Submit a job run
databricks jobs run-now --job-id 123 --notebook-params '{"date": "2024-01-01"}'
# Get run details
databricks runs get --run-id 456
# List runs for a job
databricks runs list --job-id 123 --limit 10
# Wait for a run to complete
databricks runs wait --run-id 456
# Create a cluster
databricks clusters create --spark-version 13.3.x-scala2.12 --node-type-id i3.xlarge --num-workers 2 --cluster-name my-cluster
```
### Polling for Job Completion in Python
```python
import requests
import time
token = "dapi1234567890abcdef"
workspace_url = "https://myworkspace.cloud.databricks.com"
headers = {"Authorization": f"Bearer {token}"}
# Submit a job run
response = requests.post(
    f"{workspace_url}/api/2.1/jobs/runs/submit",
    headers=headers,
    json={"job_id": 123}
)
run_id = response.json()["run_id"]
print(f"Submitted run {run_id}")
# Poll for completion
while True:
    status_response = requests.get(
        f"{workspace_url}/api/2.1/jobs/runs/get?run_id={run_id}",
        headers=headers
    )
    run_status = status_response.json()["state"]
    
    if run_status == "TERMINATED":
        result_state = status_response.json()["state_message"]
        print(f"Run completed with state: {result_state}")
        break
    
    print(f"Run state: {run_status}")
    time.sleep(10)
```
---
## Common Exam Scenarios
Scenario: You need to trigger a data pipeline from an external system and wait for it to complete before proceeding. What's the most efficient approach?
Use the Jobs API to submit a run via POST /api/2.1/jobs/runs/submit. Capture the run ID from the response. Then poll GET /api/2.1/jobs/runs/get with the run ID every 10-30 seconds until the state is TERMINATED. Check the state_message to see if it SUCCEEDED or FAILED. Alternatively, configure a webhook on the job so Databricks notifies your system when the run completes.
Scenario: You want to list all failed job runs in the past 24 hours for a compliance audit. How would you do this?
Use the Runs API with filters. Call GET /api/2.1/jobs/runs/list with parameters for job_id and a time range. Then filter the results for runs with state FAILED. Iterate through paginated results if there are many runs. The response includes run details like user, start time, and error message.
Scenario: A production system needs to start a Databricks cluster, run a job, and terminate the cluster when done. What API calls are needed?
Use the Clusters API to start: POST /api/2.1/clusters/start with cluster_id. Wait for the cluster state to become RUNNING. Then use the Jobs API to run the job. Finally, use the Clusters API to terminate: POST /api/2.1/clusters/delete with cluster_id. You can automate all this with the CLI or Python API client.
---
## Key Takeaways
- REST APIs and the CLI provide programmatic access to Databricks. Use the Jobs API to submit and monitor runs, the Clusters API to manage cluster lifecycle.
- Polling means repeatedly checking run status. Webhooks are more efficient for getting real time notifications when jobs complete.
- Authenticate with a personal access token via the `Authorization: Bearer` header (REST) or in the `.databrickscfg` file (CLI).
- Combine API calls to build production workflows: submit runs, poll for completion, extract results, and trigger downstream actions.
---
## Gotchas and Tips
Polling too frequently (e.g., every second) wastes API rate limits and is inefficient. Wait 10-30 seconds between polls. Use exponential backoff if you hit rate limits.
Personal access tokens have limited lifetime. Store them securely (not in code). Use service principals for long lived automation.
The CLI requires the Databricks Python SDK to be installed and a valid config file. Test your setup before running production scripts.
---
## Links and Resources
- **[Lakeflow Jobs](https://docs.databricks.com/aws/en/jobs/)** — Using the Jobs REST API and CLI for programmatic job management and monitoring.
- **[Databricks CLI](https://docs.databricks.com/aws/en/dev-tools/cli/)** — Command-line interface for managing Databricks resources and monitoring.