## Topic Overview
Data pipelines fail. Sometimes a step times out, a dependency is unavailable, or a data quality check fails. Rather than rerunning the entire job from scratch, Databricks allows you to repair a failed job run by retrying only the failed tasks. You can also override parameters at run time to test fixes without modifying the job definition. This capability is critical for production reliability and cost efficiency.
The Professional exam tests your ability to use the repair run API, understand idempotent design, and apply parameter overrides strategically. You need to know when repair is safe (idempotent tasks) and when it risks data inconsistency (non idempotent operations).
## Key Concepts
**Repair Run API: Retrying Failed Job Runs**
The repair_run API retries a failed job run, skipping tasks that already succeeded. Instead of rerunning all tasks, only the failed ones execute. This saves time and resources. Use the Databricks Jobs API or CLI to call repair_run with the run_id. The API allows you to specify which tasks to retry or repair the entire run.
**Parameter Overrides at Run Time**
When you trigger a job run, you can override parameters defined in the job configuration. For example, if the job takes a date parameter and defaults to today, you can run it for yesterday's date by passing a different value. This allows testing, backfilling, and ad hoc execution without changing the permanent job definition. Parameter overrides are specified as JSON in the notebook_params or spark_submit_params.
**Idempotent Task Design and Safety Implications**
A task is idempotent if running it multiple times produces the same result as running it once. For example, a task that deletes and recreates a table is idempotent. A task that appends to a table without deduplication is not idempotent and will create duplicates if repaired. Design tasks to be idempotent by using REPLACE or merge operations instead of raw APPEND. If a task is not idempotent, repair is risky and may create data inconsistencies.
**Task level vs. Run level Repairs**
You can repair a single task, a set of tasks, or the entire job run. Repairing a single task is safer because it limits scope, but requires understanding task dependencies. If task B depends on task A and task B failed, repairing only task B may not help if task A's output was incomplete. Repair the entire job run to ensure all dependencies are satisfied.
## Code Examples
### Using the Repair Run API
```python
from databricks.sdk import WorkspaceClient
client = WorkspaceClient()
# Get details of a failed run
run = client.jobs.get_run(run_id=123456)
print(f"Run state: {run.state}")
print(f"Job ID: {run.job_id}")
# Repair the entire run (retry all failed tasks)
repaired_run = client.jobs.repair_run(run_id=123456)
print(f"Repaired run ID: {repaired_run.run_id}")
# Repair specific tasks only
repaired_run = client.jobs.repair_run(
    run_id=123456,
    rerun_tasks=['task_1', 'task_2']
)
print(f"Rerunning specific tasks in run: {repaired_run.run_id}")
```
### Overriding Parameters at Job Submission
```python
from databricks.sdk import WorkspaceClient
import datetime
client = WorkspaceClient()
# Submit a job run with parameter overrides
run = client.jobs.submit_run(
    job_id=789,
    notebook_params={
        'processing_date': '2025-02-15',
        'environment': 'production',
        'target_table': 'my_catalog.my_schema.results'
    }
)
print(f"Submitted run with parameters: {run.run_id}")
# Trigger a job with Spark SQL overrides
run = client.jobs.submit_run(
    job_id=790,
    spark_submit_params=['--conf', 'spark.sql.shuffle.partitions=200']
)
print(f"Submitted run with Spark config: {run.run_id}")
# Check run status
status = client.jobs.get_run(run_id=run.run_id)
print(f"Current state: {status.state}")
```
### Writing Idempotent Task Logic
```sql
-- IDEMPOTENT: Uses REPLACE to overwrite existing data
CREATE OR REPLACE TABLE my_catalog.my_schema.daily_summary AS
SELECT date, product_id, SUM(amount) as total
FROM my_catalog.my_schema.transactions
WHERE date = CURRENT_DATE
GROUP BY date, product_id;
-- IDEMPOTENT: Uses MERGE to handle inserts and updates
MERGE INTO my_catalog.my_schema.customer_status cs
USING (
  SELECT customer_id, status
  FROM my_catalog.my_schema.processed_events
  WHERE event_date = CURRENT_DATE
) pe
ON cs.customer_id = pe.customer_id
WHEN MATCHED THEN UPDATE SET cs.status = pe.status
WHEN NOT MATCHED THEN INSERT (customer_id, status) VALUES (pe.customer_id, pe.status);
-- NOT IDEMPOTENT: Raw append creates duplicates if run twice
INSERT INTO my_catalog.my_schema.event_log
SELECT * FROM my_catalog.my_schema.raw_events
WHERE processed = false;
-- RISK: If task restarts, same rows are inserted again
```
## Common Exam Scenarios
**A multi task Lakeflow job fails on the final validation step. Tasks 1 3 completed but task 4 (validation) timed out. You want to rerun only task 4. Is this safe?**
It depends on whether task 4 depends on outputs from earlier tasks. If task 4 is a validation (SELECT only, no writes), rerunning it is safe. If task 4 writes data and relies on task 3's output, you can rerun it alone. However, if there are dependencies between tasks 1 and 3 that you don't understand, repair the entire run to be safe. The safest approach is to repair the full run, which will skip already completed tasks and retry the failed one.
**A notebook job processes daily data. Someone accidentally triggered it with yesterday's date instead of today's. You don't want to change the job definition. How do you rerun for the correct date?**
Use submit_run with notebook_params override to trigger the job again with the correct date. Pass the date parameter when submitting: notebook_params={'processing_date': '2025-02-15'}. The job will run with the overridden parameter while the permanent job definition remains unchanged. This allows ad hoc execution for backfill or recovery without modifying the job.
## Key Takeaways
- Use repair_run to retry failed tasks without rerunning the entire job, saving time and resources.
- Parameter overrides allow ad hoc execution and testing without changing the job definition.
- Idempotent task design (REPLACE, MERGE) is essential for safe repairs.
- Repair the full run unless you fully understand task dependencies.
## Gotchas and Tips
Repairing a task that appends data without idempotency creates duplicates. Always use REPLACE or MERGE for tasks that will be repaired.
Parameter overrides are task specific, not job specific. If a job has multiple notebooks, only the notebook executed during the submit_run call receives the parameter overrides.
---
## Links and Resources
- **[Lakeflow Jobs](https://docs.databricks.com/aws/en/jobs/)** — Repairing failed job runs and overriding parameters for re-execution.