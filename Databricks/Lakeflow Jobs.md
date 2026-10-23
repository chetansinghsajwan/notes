**Origin:** Lakeflow Jobs is formerly known as Databricks Workflows — same product, rebranded in 2025.
---
Lakeflow Jobs is workflow automation for Databricks, providing orchestration for data processing workloads so you can coordinate and run multiple tasks as part of a larger workflow.
**Simple analogy:** It's Databricks' built-in **Airflow** — but fully managed, no infrastructure needed, and deeply integrated with the lakehouse.
---
**3 core concepts:**
|Concept|What it is|
|---|---|
|**Job**|The top-level unit — a collection of tasks with a schedule/trigger|
|**Task**|A single unit of work within a job — a notebook, script, SQL query etc.|
|**Trigger**|What starts the job — schedule, file arrival, table update, or manual|
---
**Supported task types:**
|Task type|Example|
|---|---|
|Notebook|Run a Databricks notebook|
|Python script|Run a `.py` file|
|SQL|Run a SQL query|
|Lakeflow SDP Pipeline|Trigger a declarative pipeline|
|dbt|Run dbt models|
|Spark JAR|Run a Java/Scala JAR|
|For Each|Loop over a list of items|
|Run Job|Trigger another job (job modularity)|
|Publish Dashboard|Refresh and publish an AI/BI dashboard|
---
**Key features:**
|Feature|Detail|
|---|---|
|Control flow|If/else branching and for-each loops using a visual authoring UI|
|Triggers|Schedule (cron), file arrival, Delta table change, or on-demand|
|Repair runs|Re-run only failed tasks — not the entire job|
|Retry logic|Auto-retry failed tasks with configurable attempts|
|Task dependencies|Define which tasks run after which|
|Compute|Serverless (default) or classic cluster per task|
|Observability|Built-in run history, logs, alerts|
|SLA monitoring|Get notified if a job runs longer than expected|
---
**Lakeflow Jobs vs Airflow:**
||Lakeflow Jobs|Apache Airflow|
|---|---|---|
|Managed|Fully — no setup|Self-hosted or managed (MWAA, Astronomer)|
|Databricks integration|Native — zero config|Requires Databricks operator plugin|
|Visual UI|Yes|Yes (limited)|
|Code-first (DABs)|Yes|Yes (Python DAGs)|
|Cost|Included in Databricks|Separate infra cost|
|Best for|Databricks-native workloads|Multi-platform orchestration|
---
**In short:** Lakeflow Jobs = Databricks' native, fully managed orchestrator for scheduling, chaining, and monitoring your ETL, ML, and analytics workflows — all within the lakehouse platform, no external tool needed.