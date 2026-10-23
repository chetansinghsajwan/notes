Databricks provides two types of computes:
[[Databricks/Compute/Serverless Compute|Databricks Serverless Compute]]
[[Classic Compute|Databricks Classic Compute]]
---
Databricks compute refers to the selection of computing resources available on Databricks to run data engineering, data science, and analytics workloads. In simple terms — it's the **cluster of machines** that actually runs your code.
---
**3 main compute types:**
|Compute Type|Best for|Managed by|Cost level|
|---|---|---|---|
|**Serverless**|Notebooks, jobs, pipelines, SQL|Fully by Databricks|Pay per use — no idle cost|
|**Classic (All-Purpose)**|Interactive notebooks, exploration|You (manual start/stop)|Highest — runs until stopped|
|**SQL Warehouse**|BI dashboards, Databricks SQL queries|Databricks|Medium|
---
**Deeper breakdown:**
**1. Serverless Compute**
- No infrastructure to manage — Databricks spins up and tears down automatically.
- Available for: notebooks, jobs, pipelines (Lakeflow SDP), SQL.
- On-demand, automatically managed compute that scales based on workload requirements.
- **Recommended default** as of 2026.
**2. Classic Compute (Clusters)**
|Sub-type|Used for|Cost multiplier|
|---|---|---|
|All-Purpose Cluster|Interactive notebooks, ad-hoc work|Highest (~3–4x jobs)|
|Jobs Cluster|Automated scheduled tasks like ETL pipelines — spins up for a job and shuts down when done|Lowest|
**3. SQL Warehouse**
- Optimized specifically for SQL analytics and BI tools.
- Three sub-types: Serverless, Pro, Classic.
- Powers Databricks SQL editor, dashboards, and external BI tools like Power BI, Tableau.
---
**Key concepts within compute:**
|Concept|What it means|
|---|---|
|Driver node|Master node — coordinates the cluster, runs your notebook|
|Worker nodes|Execute actual Spark tasks|
|DBU|Databricks Unit — the billing unit for compute usage|
|Autoscaling|Cluster adds/removes workers based on load|
|Spot instances|Cheaper cloud VMs — can be interrupted (good for jobs, risky for interactive)|
|Photon|Optional — enable for faster SQL/DataFrame performance|
|Runtime (DBR)|Databricks Runtime version — pre-bundled Spark + libraries|
---
**Cost tip for India:**  
Running the same code on an All-Purpose cluster can cost 3 to 4 times more in DBUs than running it as an automated job on a Jobs cluster. Always use **Jobs Compute** for production pipelines, never All-Purpose.