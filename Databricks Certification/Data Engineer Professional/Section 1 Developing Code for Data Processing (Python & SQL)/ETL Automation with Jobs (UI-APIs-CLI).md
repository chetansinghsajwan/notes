## Topic Overview
Lakeflow Jobs (formerly Databricks Workflows) are the way to orchestrate and automate data pipelines in production. A job is a set of tasks that can depend on each other, can be triggered on a schedule, and can be monitored centrally. Instead of manually running notebooks or Python scripts, jobs handle scheduling, retries, error notifications, and logging automatically.
You can create and manage jobs through the Databricks UI, the REST API, or the CLI. The REST API and CLI approaches are especially important for production because they let you define jobs in code (infrastructure as code). Multi task jobs let you orchestrate complex workflows where some tasks run in parallel and others depend on earlier tasks. This level of control is essential for production data pipelines.
The Professional exam focuses on creating jobs via API and CLI, understanding task dependencies, parameterized jobs, and choosing between job clusters and all purpose clusters. You need to know how to trigger jobs, repair failed runs, and override parameters.
---
## Key Concepts
Jobs vs Lakeflow Pipelines
Lakeflow Spark Declarative Pipelines are best for complex ETL where you define tables and views with automatic dependency tracking. Jobs are more general orchestration tools. A job can run notebooks, Python scripts, jars, or SQL queries. Jobs provide more flexibility for arbitrary workflows. Lakeflow pipelines are specifically optimized for data transformations with quality checks and incremental updates. In practice, you often use Lakeflow pipelines for the data transformations and wrap them in a job for scheduling and monitoring.
Task Dependencies and Multi-Task Jobs
A multi task job is a job with multiple tasks that can run in sequence or in parallel. Each task can depend on other tasks, creating a DAG (directed acyclic graph) of work. For example, Task A might ingest data, Task B and Task C might do independent transformations in parallel, and Task D might combine the results. You define dependencies using the depends_on field in task configuration. This is more powerful than a single task because it lets you express complex workflows and parallelize work where possible.
Job Clusters vs All Purpose Clusters
Job clusters are created when the job starts and terminated when the job completes. This is cost effective for batch jobs because you only pay for compute during job execution. All purpose clusters persist and are used by multiple users or jobs. Use job clusters for production batch jobs. Use all purpose clusters for interactive development or when multiple jobs share a cluster. For DABs, job clusters are the preferred approach because they are isolated, reproducible, and cost effective.
Parameterized Jobs
Jobs can accept parameters that are passed to tasks at runtime. Parameters make jobs reusable and flexible. For example, a data ingestion job might accept a date range parameter so you can rerun it for different time periods without changing the code. You define parameters in the job config and access them in your code via task parameters. This is essential for maintaining a single job definition that can handle multiple scenarios.
---
## Code Examples
### Multi-Task Job via REST API
```python
# Create a multi task job using the Databricks REST API
import requests
import json
api_url = "https://<instance>.cloud.databricks.com/api/2.1/jobs/create"
headers = {"Authorization": f"Bearer {token}"}
job_config = {
    "name": "ETL Pipeline",
    "tasks": [
        {
            "task_key": "ingest",
            "notebook_task": {
                "notebook_path": "/Workspace/etl/ingest",
                "source": "WORKSPACE"
            },
            "job_cluster_key": "compute",
            "max_retries": 2,
            "timeout_seconds": 3600
        },
        {
            "task_key": "transform",
            "notebook_task": {
                "notebook_path": "/Workspace/etl/transform"
            },
            "depends_on": [{"task_key": "ingest"}],
            "job_cluster_key": "compute"
        },
        {
            "task_key": "publish",
            "notebook_task": {
                "notebook_path": "/Workspace/etl/publish"
            },
            "depends_on": [{"task_key": "transform"}],
            "job_cluster_key": "compute"
        }
    ],
    "job_clusters": [
        {
            "job_cluster_key": "compute",
            "new_cluster": {
                "spark_version": "13.3.x-scala2.12",
                "node_type_id": "i3.xlarge",
                "num_workers": 2
            }
        }
    ],
    "schedule": {
        "quartz_cron_expression": "0 0 * * * ?",
        "timezone_id": "UTC"
    }
}
response = requests.post(api_url, json=job_config, headers=headers)
job_id = response.json()["job_id"]
print(f"Created job {job_id}")
```
### Parameterized Job via CLI
```bash
# Create a job with parameters using the Databricks CLI
databricks jobs create --json '{                                           
  "name": "Data Backfill",
  "tasks": [
    {
      "task_key": "backfill",
      "notebook_task": {
        "notebook_path": "/Workspace/jobs/backfill_data",
        "base_parameters": {
          "start_date": "2024-01-01",
          "end_date": "2024-01-31"
        }
      },
      "new_cluster": {
        "spark_version": "13.3.x-scala2.12",
        "node_type_id": "i3.xlarge",
        "num_workers": 2
      }
    }
  ]
}'
# Trigger the job with different parameters
databricks jobs run-now \
  --job-id 123 \
  --notebook-params '{"start_date": "2024-02-01", "end_date": "2024-02-29"}'
```
### Repair a Failed Job Run
```bash
# Get the job run ID and repair it
databricks runs list --job-id 123 --limit 1
# Repair the failed run (rerun from the point of failure)
databricks runs repair-run --run-id 456
# Repair a specific failed task within a multi task job
databricks runs repair-run --run-id 456 --task-key transform
```
---
## Common Exam Scenarios
A data pipeline has three stages: ingest raw data, validate and clean it, then publish to a data mart. The team wants these to run in sequence on a schedule, with automatic retries if any step fails. How should this be set up? Create a multi task job with three tasks (ingest, validate, publish) where validate depends on ingest and publish depends on validate. Use a job cluster to keep costs low and set max_retries on each task. This is cleaner and more reliable than running three separate scheduled jobs.
A backfill job needs to reprocess data for a specific date range, but the pipeline code itself is generic. Should the same job definition be reused for different date ranges? Yes, use a parameterized job. Define parameters for start_date and end_date in the job config, then trigger the job with different parameter values for different date ranges. This way, you maintain a single job definition in code and avoid duplication.
A job run failed partway through a multi task pipeline. The engineer wants to retry from the point of failure without rerunning all earlier tasks. What command should they use? Use databricks runs repair-run --run-id <run_id> to rerun only the failed task and its downstream dependents. You can optionally specify a task_key to repair only that specific task. This is more efficient than rerunning the entire job.
---
## Key Takeaways
- Use Lakeflow Jobs to orchestrate multi task pipelines with dependencies and scheduling.
- Define task dependencies using the depends_on field to create complex workflows.
- Use job clusters for production batch jobs to minimize costs.
- Parameterize jobs to reuse the same definition for different inputs or date ranges.
- Create and manage jobs via REST API or CLI for infrastructure as code practices.
---
## Gotchas and Tips
⚠️ Job clusters take time to start up. If you have many short tasks, the startup overhead might dominate. For very fast tasks, consider using an all purpose cluster instead.
⚠️ When defining task dependencies, be careful not to create circular dependencies. Databricks will reject job configs with circular dependencies.
⚠️ Parameters are passed to tasks as environment variables or notebook widgets depending on the task type. Always check how your task type receives parameters before using them.
---
## Links and Resources
- **[Lakeflow Jobs](https://docs.databricks.com/aws/en/jobs/)** — Orchestrating ETL workflows with Databricks Jobs, including UI, API, and CLI methods.
- **[Job Notifications](https://docs.databricks.com/aws/en/jobs/notifications)** — Configuring alerts and notifications for job run events.