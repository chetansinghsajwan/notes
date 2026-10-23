## Topic Overview
Databricks offers several types of compute, and picking the right one for the job is a common exam topic. The three main categories are **All Purpose Clusters**, **Jobs Clusters**, and **SQL Warehouses**. Each is designed for specific workloads, and choosing the wrong one wastes money or delivers poor performance.
The exam will present scenarios and ask you to identify which compute type is most appropriate. The key is to match the workload type (interactive development, scheduled production jobs, SQL analytics) to the right compute resource. You should also know about **Serverless compute**, which is Databricks' managed option that removes the need to configure and manage clusters yourself.
This is one of those topics where understanding the trade offs is more important than knowing deep technical details. Think about cost, startup time, who the user is, and what they are trying to do.
---
## Key Concepts
**All Purpose Clusters (Interactive Clusters)**
All Purpose Clusters are designed for interactive workloads: developing code in notebooks, running ad hoc queries, exploring data, and collaborating with teammates. They stay running until you manually terminate them (or an auto termination policy kicks in). Multiple users can attach to the same cluster and run notebooks simultaneously. These clusters support Python, SQL, Scala, and R. Because they stay alive between runs, you pay for the entire time the cluster is up, even if nobody is actively running code. This makes them more expensive for production workloads compared to Jobs Clusters.
**Jobs Clusters**
Jobs Clusters are created automatically when a scheduled job or workflow starts, and they are terminated as soon as the job completes. You do not interact with them directly. They are optimized for production workloads: ETL pipelines, scheduled reports, model training runs, and anything that runs on a schedule or is triggered by an event. Because they spin up only when needed and shut down immediately after, they are significantly cheaper than keeping an All Purpose Cluster running. Lakeflow Jobs (formerly Databricks Workflows) uses Jobs Clusters by default.
**SQL Warehouses**
SQL Warehouses are purpose built for SQL workloads: running queries, powering dashboards, and serving BI tools. They are optimized for the Photon engine, which accelerates SQL query execution. SQL Warehouses come in three sizes: Serverless, Pro, and Classic. Serverless SQL Warehouses start instantly and scale automatically. Pro warehouses add features like query caching and materialized views. Classic warehouses are the basic option. Data analysts typically work with SQL Warehouses rather than clusters because the interface and performance are tuned for SQL.
**Serverless Compute**
Serverless compute is available for both SQL Warehouses and general purpose compute (notebooks and jobs). With serverless, Databricks manages all the infrastructure. You do not choose instance types, configure autoscaling, or manage cluster policies. Resources are allocated instantly and scale based on demand. The trade off is that you have less control over the exact hardware configuration, but for most workloads, the convenience and fast startup times outweigh this. Serverless is the direction Databricks is pushing for most users.
**Photon Engine**
Photon is Databricks' native vectorized query engine written in C++. It accelerates SQL and DataFrame operations significantly compared to standard Spark. Photon is enabled by default on SQL Warehouses and can be enabled on clusters. It is especially effective for scan heavy and aggregation heavy workloads. You do not need to change your code to take advantage of Photon; it works transparently under the hood. For the exam, know that Photon is the engine that makes SQL Warehouses fast, and it is a key reason to use SQL Warehouses for analytical queries.
---
## Quick Comparison
|**Compute Type**|**Best For**|**Lifecycle**|**Cost**|**Languages**|
|---|---|---|---|---|
|All Purpose Cluster|Interactive dev, notebooks, exploration|Manual start/stop|Higher (always on)|Python, SQL, Scala, R|
|Jobs Cluster|Production ETL, scheduled jobs|Auto created/terminated|Lower (on demand)|Python, SQL, Scala, R|
|SQL Warehouse|SQL queries, dashboards, BI tools|Auto scaling|Optimized for SQL|SQL only|
|Serverless|Any workload, zero management|Fully managed|Pay per use|Depends on type|
---
## Common Exam Scenarios
### Scenario 1: Choosing Compute for a Production Pipeline
A data engineer has developed an ETL pipeline in a notebook and wants to schedule it to run every night at midnight. The pipeline processes sales data and loads it into a gold table. Which compute type should they use?
The answer is a **Jobs Cluster** (or Serverless compute for jobs). Since this is a scheduled production workload, you want compute that spins up when the job starts and terminates when it finishes. Using an All Purpose Cluster for a scheduled nightly job would mean either keeping it running 24/7 (expensive) or dealing with startup delays. Jobs Clusters are created automatically by the scheduler and terminated immediately after completion.
### Scenario 2: Powering a BI Dashboard
The analytics team needs to connect Tableau to Databricks and run interactive SQL queries against gold layer tables. They need fast query response times and want the compute to scale based on the number of concurrent users.
The answer is a **SQL Warehouse** (preferably Serverless). SQL Warehouses are purpose built for this use case. They support JDBC/ODBC connections (which Tableau uses), they run the Photon engine for fast SQL execution, and they auto scale based on query load. An All Purpose Cluster could technically work, but it would not scale as smoothly and the Photon engine is more tightly integrated with SQL Warehouses.
### Scenario 3: Developing and Testing a New Pipeline
A data engineer is building a new pipeline from scratch. They need to interactively write code, test transformations, inspect intermediate results, and iterate quickly. They will be working in Python and SQL notebooks.
The answer is an **All Purpose Cluster** (or Serverless compute for notebooks). Interactive development requires a long running compute resource that stays available while the engineer works. All Purpose Clusters keep their state between cell executions, which means variables, DataFrames, and temporary views persist throughout the development session. Once the pipeline is ready for production, the engineer should switch to a Jobs Cluster for the scheduled runs.
---
## Key Takeaways
- **All Purpose Clusters** are for interactive development and exploration. They stay running and support multiple users. Use them when building and testing, not for production workloads.
- **Jobs Clusters** are for production workloads. They are created when a job starts and terminated when it finishes. Always prefer Jobs Clusters (or Serverless) for scheduled pipelines.
- **SQL Warehouses** are optimized for SQL workloads, BI tools, and dashboards. They run Photon for fast query execution and support JDBC/ODBC connections.
- **Serverless compute** eliminates infrastructure management. It starts instantly, scales automatically, and is available for notebooks, jobs, and SQL Warehouses.
- The exam decision tree: Interactive dev → All Purpose Cluster. Scheduled jobs → Jobs Cluster. SQL/BI → SQL Warehouse. Want zero management → Serverless.
---
## Gotchas and Tips
SQL Warehouses only support SQL. If a question mentions running Python or Scala code, SQL Warehouses are not the answer. You need an All Purpose Cluster or Jobs Cluster for those languages.
A common trap: using an All Purpose Cluster for a production pipeline because "it works." Yes, it works, but it is not cost effective. The exam expects you to know that Jobs Clusters are the right choice for production. The answer is almost never "use an All Purpose Cluster" for scheduled workloads.
Serverless SQL Warehouses start in seconds, while classic SQL Warehouses and clusters can take several minutes to start. If a question emphasizes fast startup time or instant availability, Serverless is likely the answer.
Cluster policies are used by admins to control what types of clusters users can create. If a question asks about enforcing compute standards across a team (limiting instance types, max nodes, auto termination), the answer involves cluster policies. This is related to governance and cost control.
---
## Links and Resources
- **[SQL warehouse sizing, scaling, and queuing](https://docs.databricks.com/aws/en/compute/sql-warehouse/warehouse-behavior)** — How SQL warehouses scale and handle concurrent queries.
- **[Configure compute for jobs](https://docs.databricks.com/aws/en/jobs/compute)** — Choosing between classic and serverless compute for job workloads.
- **[Run jobs with serverless compute](https://docs.databricks.com/aws/en/jobs/run-serverless-jobs)** — Serverless compute for workflows with automatic scaling.
- **[Serverless compute for notebooks](https://docs.databricks.com/aws/en/compute/serverless/notebooks)** — Using serverless compute for interactive notebook workloads.