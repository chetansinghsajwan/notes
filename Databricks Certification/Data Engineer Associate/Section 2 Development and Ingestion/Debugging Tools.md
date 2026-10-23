## Topic Overview
Databricks provides several built in debugging tools for data engineers to understand, troubleshoot, and optimize their data pipelines. These tools give you visibility into what's happening at every layer of your application, from cluster resource usage to individual task execution times. When something goes wrong, having quick access to the right debugging information can save you hours of guesswork.
The exam expects you to know which tool to reach for in different scenarios. If you need to understand why a Spark job is slow, you'll want the Spark UI. If you need to see what a notebook cell printed out, you check the cell output. If you're debugging a cluster configuration issue, you look at the cluster event log. Understanding the purpose and access point for each tool is critical.
The good news is that most of these tools are already available in the Databricks interface. You don't need to install anything or write complicated logging code. They're just waiting for you to find them and use them.
---
## Key Concepts
**Spark UI and Execution Plans**
The Spark UI is your window into how Spark executes your jobs. You access it from the cluster details page in the Spark UI tab. It shows you job stages, task distribution across executors, shuffle operations, memory usage, and execution plans. Each stage maps to a transformation in your code, and you can see exactly which tasks ran, how long they took, and whether they spilled data to disk. When a job runs slower than expected, the Spark UI often reveals the bottleneck.
**Driver and Executor Logs**
The driver node runs the main Python or Scala process of your notebook or job. Its stdout and stderr output get captured in driver logs. You'll find these in the cluster's Driver Logs tab. They're useful for seeing print statements, exception tracebacks, application level log messages, and any output from your code that doesn't go to a specific table or file. If your notebook crashes during initialization or your Python code throws an exception, the driver logs show you why.
**Cluster Event Logs**
Every cluster generates an event log that records its lifecycle. When the cluster starts, stops, restarts, resizes, or encounters an issue, the event log captures it with a timestamp and details. You can view this log in the cluster details page under the Events tab. If a job keeps failing because the cluster keeps terminating, the event log tells you when and why the cluster went down.
**Notebook Debugging Features**
Each cell in a notebook displays its output directly below it. If a cell errors, you see a red error message with a stack trace and the line number where the error occurred. The `display()` function renders data beautifully in tabular or visual form. The `dbutils.notebook.exit()` function lets you exit a notebook and pass a value back to a calling workflow, useful for signaling success or failure. These features make it easy to debug as you write code.
**Query Profile for SQL**
When you run SQL queries in a SQL Warehouse or on a cluster with SQL support, Databricks generates a query execution profile. This shows you a visual representation of the query plan, which tables are scanned, how data is joined, and what operations consume the most time and resources. The query profile is similar to the Spark UI but tailored for SQL. It helps you identify inefficient joins, missing indexes, or unnecessary full table scans.
**System Tables for Observability**
Databricks exposes system tables in the `system` schema that let you query metadata and audit logs programmatically. The `system.access.audit` table tracks who accessed what and when. The `system.compute.clusters` table contains information about cluster configurations, sizes, and states. Using SQL queries against these tables, you can build custom dashboards and alerts. This is especially useful on the Associate exam for questions about audit logs and cluster diagnostics.
**Lakeflow Pipeline Event Logs**
When you use Lakeflow Spark Declarative Pipelines (the declarative approach to building data pipelines), each pipeline execution generates event logs. These logs track when tables were refreshed, how many rows were processed, whether any errors occurred, and how long each stage took. You can access pipeline event logs from the Lakeflow Pipelines UI or query them directly. They're invaluable for debugging failed pipeline runs and understanding data lineage.
---
## Code Examples
### Using display() and print() for Debugging in Python
```python
# Using print() for simple output (shows in cell output and driver logs)
print("Processing started at", spark.sql("SELECT current_timestamp()").collect()[0][0])
# Using display() to render a DataFrame beautifully
df = spark.read.table("bronze_customers")
display(df.limit(100))
# Debugging with print() and a condition
row_count = df.count()
print(f"Total rows in bronze_customers: {row_count}")
if row_count == 0:
    print("WARNING: Table is empty!")
# Using display() with SQL for interactive exploration
display(spark.sql("SELECT * FROM bronze_customers WHERE status = 'ACTIVE' LIMIT 50"))
```
### Querying System Tables for Cluster Diagnostics in SQL
```sql
-- Check cluster configurations and current states
SELECT cluster_id, cluster_name, state, worker_count, driver_node_type_id
FROM system.compute.clusters
WHERE state = 'RUNNING'
ORDER BY cluster_name;
-- Audit log: who accessed what and when
SELECT timestamp, user_identity.email as user_email, action, object_type, object_id
FROM system.access.audit
WHERE action LIKE '%READ%' OR action LIKE '%WRITE%'
ORDER BY timestamp DESC
LIMIT 100;
-- Find recent access to a specific table
SELECT timestamp, user_identity.email, action
FROM system.access.audit
WHERE object_type = 'TABLE' AND object_id = 'silver_orders'
ORDER BY timestamp DESC
LIMIT 50;
```
### Using try/except for Error Handling and Debugging in Python
```python
import traceback
# Basic try/except with print debugging
try:
    df = spark.read.csv("/path/to/data.csv", header=True)
    result = df.groupBy("category").count()
    display(result)
except FileNotFoundError as e:
    print(f"File not found: {e}")
except Exception as e:
    print(f"Unexpected error: {e}")
    print(traceback.format_exc())  # Full stack trace in driver logs
# Debugging data validation
try:
    df = spark.read.table("orders")
    null_count = df.filter(df.order_id.isNull()).count()
    if null_count > 0:
        print(f"WARNING: Found {null_count} null order_ids")
    else:
        print("All order_ids are valid")
except Exception as e:
    print(f"Error reading table: {e}")
    traceback.print_exc()
```
### Using dbutils.notebook.exit() for Workflow Signaling
```python
# Success case: exit with a success message
print("Pipeline completed successfully")
row_count = spark.read.table("gold_metrics").count()
print(f"Loaded {row_count} rows to gold layer")
dbutils.notebook.exit(f"Success: {row_count} rows processed")
# Error case: exit with an error status
try:
    df = spark.read.table("critical_data")
    if df.count() == 0:
        error_msg = "Source table is empty - cannot proceed"
        print(f"ERROR: {error_msg}")
        dbutils.notebook.exit(error_msg)
except Exception as e:
    error_msg = f"Failed to read table: {str(e)}"
    print(f"ERROR: {error_msg}")
    dbutils.notebook.exit(error_msg)
```
---
## Common Exam Scenarios
**Scenario 1: A Spark job runs but takes much longer than expected**
You would access the Spark UI to examine the job stages and tasks. Look for stages with long durations or high memory usage. Check whether tasks are well distributed across executors or if some executors are doing much more work than others. Look for stages with large shuffle operations, which are often the bottleneck. You might also see if tasks are spilling to disk, which causes dramatic slowdowns. The Spark UI will show you exactly where the time is being spent so you can optimize the right part of your code.
**Scenario 2: A Python notebook fails with a cryptic error message in a cell**
First, you check the cell output directly below the cell. Databricks shows a red error box with the exception type and message. If the message is still unclear, you check the driver logs in the cluster details page. The driver logs contain the full stack trace, showing you the sequence of function calls that led to the error. You look at the line numbers in the traceback and review your code. Often, the actual issue is several calls up the stack from where the error was raised. The combination of cell output and driver logs gives you the full picture.
**Scenario 3: You need to audit which users accessed a specific table and when**
You query the `system.access.audit` table with a filter for the table name and action type. The audit table contains a timestamp, user email, action (READ, WRITE, etc.), object type, and object ID. You can join this with other system tables or aggregate the results to answer compliance questions like how many times was this table queried today or which users have accessed this sensitive table. Using system tables lets you build compliance reports and answer governance questions without manual log searching.
---
## Key Takeaways
- The Spark UI shows job execution details like stages, tasks, memory usage, and shuffle operations. Access it from the cluster details page to debug slow jobs.
- Driver logs capture all stdout and stderr from your notebook or job, including print statements, exceptions, and stack traces.
- Notebook cell output shows errors with line numbers and stack traces, making it easy to locate and fix bugs as you develop.
- System tables like `system.access.audit` and `system.compute.clusters` let you query metadata and audit logs for custom dashboards and compliance reports.
- Use `dbutils.notebook.exit()` to signal success or failure from a notebook when it's called as part of a workflow.
---
## Gotchas and Tips
**Driver logs are for driver side activity only.** If your Spark job is running on executors (which is most of the time), the driver logs won't show the details of that distributed execution. That's what the Spark UI is for. The driver logs are for print statements in your notebook and errors in your Python initialization code.
**System tables require the right permissions.** You can't query `system.access.audit` unless you have the right Unity Catalog permissions. On the exam, know that these tables exist and what they contain, but don't assume you can always query them. In restricted environments, system table access is tightly controlled for security and compliance reasons.
**The Spark UI is temporary.** Once a cluster terminates, the Spark UI for that cluster's jobs goes away. If you need long term logging of job performance, query the event logs or use system tables. For production workflows, rely on event logs and custom dashboards rather than hoping to check the Spark UI later.
---
## Links and Resources
- **[Debugging with the Spark UI](https://docs.databricks.com/aws/en/compute/troubleshooting/debugging-spark-ui)** — Step by step guide to diagnosing issues with the Spark UI.
- **[Diagnose cost and performance issues using the Spark UI](https://docs.databricks.com/aws/en/optimizations/spark-ui-guide/)** — Comprehensive guide to identifying bottlenecks and optimizing Spark jobs.