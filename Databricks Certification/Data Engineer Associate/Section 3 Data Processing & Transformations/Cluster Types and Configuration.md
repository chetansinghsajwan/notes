## Topic Overview
Databricks provides multiple cluster types and compute options tailored to different workloads and use cases. Understanding when to use each cluster type is critical for the exam and for building efficient data pipelines. The main distinction is between **All Purpose Clusters** (interactive, long running), **Job Clusters** (ephemeral, job specific), and **SQL Warehouses** (SQL optimized). Beyond these, **Serverless Compute** removes the need for manual cluster management entirely.
All Purpose Clusters are created manually or via API and persist until you explicitly terminate them. They support multiple users and multiple notebooks running simultaneously, making them ideal for interactive development, ad hoc analysis, and exploratory work. You pay for the compute hours regardless of whether you are actively running code.
Job Clusters, by contrast, are created automatically when a job starts and terminated as soon as the job finishes. They are cost effective for production workloads because you only pay for the compute time the job actually runs. Job Cluster configuration is defined within the job specification, not independently. You cannot share a Job Cluster across multiple jobs—each job gets its own ephemeral cluster.
SQL Warehouses are specialized endpoints optimized for SQL queries and BI tools. They come in three flavors—Classic, Pro, and Serverless—and leverage **Photon**, a native vectorized query engine that accelerates SQL workloads. Serverless Compute is a managed option where Databricks handles cluster provisioning automatically, providing instant startup with no configuration overhead.
## Key Concepts
All Purpose vs Job Clusters
All Purpose Clusters persist until manually terminated and can run multiple workloads simultaneously. You create them manually via the UI or API and they continue to exist (and incur costs) even when idle. Job Clusters are ephemeral and lifecycle controlled—they spin up when a job starts and terminate when the job ends. All Purpose Clusters are for interactive work; Job Clusters are for scheduled production jobs. Job Clusters are more cost effective for automated workflows.
SQL Warehouses
SQL Warehouses are dedicated compute endpoints for SQL workloads and BI tool integration. They come in three tiers: Classic (cost effective), Pro (balanced), and Serverless (fully managed). All support Photon acceleration. SQL Warehouses are optimized for query execution and are the preferred choice for dashboards, BI tools, and SQL based reporting. They are not suitable for general purpose data engineering work like cluster creation for Spark jobs.
Serverless Compute
Serverless Compute is Databricks managed compute that requires zero configuration. It is available for SQL Warehouses, notebooks, and jobs. You do not need to specify cluster size, instance types, or worker counts—Databricks handles all provisioning automatically. Startup is instantaneous. You pay per unit of compute consumed (DBUs). Serverless is ideal when you want simplicity and do not want to manage cluster infrastructure.
Autoscaling
Autoscaling allows a cluster to dynamically adjust the number of worker nodes between a defined minimum and maximum. When demand increases, new workers are added. When demand decreases, workers are removed. You configure autoscaling by specifying min workers and max workers in the cluster specification. This optimizes cost by avoiding over provisioning while ensuring adequate capacity for workload spikes.
Cluster Policies
Cluster Policies are admin defined templates that restrict cluster configuration options. They enforce organizational standards like compute limits, runtime versions, and instance types. Users can only create clusters that conform to the policy. Policies reduce sprawl, enforce cost controls, and ensure compliance with organizational requirements.
Cluster Access Modes
Databricks supports three access modes: Shared (multiple users, Unity Catalog enforced for governance), Single User (cluster dedicated to one user, full isolation), and No Isolation Shared (legacy mode, no Unity Catalog enforcement). Shared mode is the modern default and required for Unity Catalog features. Single User is used when isolation and dedicated resources are needed. No Isolation Shared is deprecated.
Photon
Photon is Databricks native vectorized query engine that dramatically speeds up SQL and DataFrame operations. It is available on specific Databricks Runtime versions. When enabled, Photon optimizes query execution by processing data in columnar format, reducing CPU and memory usage. SQL Warehouses use Photon by default. You can enable Photon on All Purpose Clusters via configuration.
Single Node vs Multi Node Clusters
Single node clusters have no worker nodes—the driver executes tasks locally. They are useful for lightweight testing, prototyping, and single machine workloads. Multi node clusters have a driver and one or more worker nodes for distributed processing. Multi node clusters are required for production workloads and large scale data processing. Single node is cheaper but not suitable for parallelized work.
Instance Types, Spot Instances, and Init Scripts
Instance types define the CPU, memory, and storage of each node (e.g., i3.xlarge). Spot instances are discounted but subject to interruption—good for fault tolerant batch jobs. Init scripts are bash scripts that run when a cluster starts, used to install libraries, configure environment variables, or install software. Databricks Runtime versions determine the Apache Spark version and available libraries.
## Code Examples
All Purpose Cluster Configuration (JSON):
```json
{
  "cluster_name": "my-interactive-cluster",
  "spark_version": "15.4.x-scala2.12",
  "node_type_id": "i3.xlarge",
  "driver_node_type_id": "i3.xlarge",
  "num_workers": 2,
  "autoscale": {
    "min_workers": 1,
    "max_workers": 4
  },
  "aws_attributes": {
    "use_spot_instances": true,
    "spot_bid_price_percent": 70
  },
  "init_scripts": [
    {
      "s3": {
        "destination": "s3://my-bucket/scripts/install-packages.sh"
      }
    }
  ],
  "access_mode": "SHARED",
  "photon_driver_node": true,
  "photon_worker_node": true
}
```
Job Cluster Configuration (via Databricks REST API concept):
```json
{
  "task_key": "process_data",
  "new_cluster": {
    "cluster_name": "job-cluster-process",
    "spark_version": "15.4.x-scala2.12",
    "node_type_id": "i3.xlarge",
    "num_workers": 3,
    "aws_attributes": {
      "use_spot_instances": true
    }
  },
  "spark_python_task": {
    "python_file": "dbfs:/scripts/etl.py"
  }
}
```
SQL Query on SQL Warehouse with Photon:
```sql
-- Query executed on SQL Warehouse (Photon enabled)
SELECT 
    customer_id,
    SUM(order_amount) as total_spent,
    COUNT(*) as order_count,
    AVG(order_amount) as avg_order
FROM orders
WHERE order_date >= '2024-01-01'
GROUP BY customer_id
ORDER BY total_spent DESC
LIMIT 100;
```
Programmatically checking cluster status (conceptual Python):
```python
# Using Databricks API to get cluster info
import requests
cluster_id = "1234-567890-test123"
response = requests.get(
    f"https://my-workspace.cloud.databricks.com/api/2.0/clusters/get",
    params={"cluster_id": cluster_id},
    headers={"Authorization": f"Bearer {token}"}
)
cluster_info = response.json()
print(f"State: {cluster_info['state']}")
print(f"Driver node type: {cluster_info['driver_node_type_id']}")
print(f"Number of workers: {cluster_info['num_workers']}")
```
## Common Exam Scenarios
**Scenario 1: Choosing the Right Cluster for a Scheduled ETL Job**
You are designing a production ETL pipeline that runs every night at midnight. The job processes 500 GB of data using PySpark and takes about 30 minutes. Management wants to minimize compute costs. Which cluster type should you use and why? The answer is a **Job Cluster**. Job Clusters are created when the job starts and terminated when it finishes, so you only pay for the 30 minutes of actual computation time. An All Purpose Cluster would be wasteful because it would persist for 23.5 hours after the job completes, incurring charges even while idle. Serverless Compute is also a valid option if cost is the priority and you want to avoid cluster management entirely.
**Scenario 2: Interactive Development with Multiple Notebooks**
A data scientist is exploring a large dataset and needs to run multiple notebooks simultaneously while iterating on SQL queries and PySpark code. They need the cluster to remain available for several hours. Which cluster type should they use? The answer is an **All Purpose Cluster**. All Purpose Clusters support multiple users and long running workloads. They persist until manually terminated, making them ideal for interactive development. A Job Cluster would terminate immediately after the first notebook completes, interrupting the workflow.
**Scenario 3: SQL Dashboard Queries Timing Out**
A BI team has set up dashboards that query a large fact table with millions of rows. The queries are slow and frequently time out. The team wants to improve query performance without increasing infrastructure costs. What is the best solution? The answer is to use a **SQL Warehouse with Photon enabled**. SQL Warehouses are optimized for SQL workloads and BI tools. Photon is a vectorized query engine that dramatically accelerates SQL execution. SQL Warehouses come in Serverless, Pro, and Classic tiers, providing options for different budgets. This is the intended use case for SQL Warehouses.
## Key Takeaways
- Use **Job Clusters** for production scheduled workloads to minimize cost—they only run while the job executes.
- Use **All Purpose Clusters** for interactive development, prototyping, and collaborative work where you need the cluster to persist.
- Use **SQL Warehouses** with Photon for SQL heavy workloads, dashboards, and BI integrations.
- **Autoscaling** optimizes cost by dynamically adjusting worker count based on demand—configure min and max workers.
- **Serverless Compute** eliminates cluster configuration burden—Databricks manages provisioning, startup is instant, and you pay per compute unit.
## Gotchas and Tips
Job Clusters Cannot Be Shared. If you define a Job Cluster within a job specification, that cluster exists only for that job. You cannot reuse the same Job Cluster definition across multiple jobs. Each job gets its own ephemeral cluster. If you want to run multiple jobs on the same persistent cluster, use an All Purpose Cluster and configure the jobs to use it.
Spot Instances Have Interruption Risk. While spot instances are cheaper (often 50-70% discount), cloud providers can reclaim them with minimal notice. Use spot instances for fault tolerant, retryable batch jobs. Avoid them for long running interactive sessions where interruption would be disruptive. You can mix spot and on demand instances on the same cluster for balance.
All Purpose Clusters Keep Running and Accruing Costs Until Explicitly Terminated. Unlike Job Clusters which auto terminate, All Purpose Clusters persist indefinitely after creation. If you forget to terminate a cluster at the end of your day, you will be charged for the entire night. Always terminate interactive clusters when you are done. Some workspaces implement auto terminate policies after a period of inactivity (e.g., 30 minutes).
---
## Links and Resources
- **[SQL warehouse sizing, scaling, and queuing](https://docs.databricks.com/aws/en/compute/sql-warehouse/warehouse-behavior)** — How SQL warehouses behave under load including auto scaling.
- **[Configure compute for jobs](https://docs.databricks.com/aws/en/jobs/compute)** — Selecting the right compute type for different job workloads.
- **[Connect to serverless compute](https://docs.databricks.com/aws/en/admin/workspace-settings/serverless)** — Enabling and configuring serverless compute in your workspace.