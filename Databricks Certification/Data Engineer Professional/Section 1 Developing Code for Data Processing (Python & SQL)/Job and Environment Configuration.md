## Topic Overview
Job and environment configuration controls how and where your code runs. At the cluster level, you define instance types, node counts, Spark configurations, and libraries. At the job level, you set timeout, retries, and resource constraints. Lakeflow Jobs use Databricks Asset Bundles (DABs) to manage these configurations as code, making pipelines reproducible and version controlled.
Key configuration decisions include choosing between new clusters (spun up per job run), existing clusters (pre running), and serverless compute (managed by Databricks). You can also control Spark settings like shuffle partitions and broadcast threshold, manage library dependencies (PyPI packages, eggs, jars), and use init scripts for custom setup.
On the exam, you will encounter questions about cluster policies for compliance, environment variables for secrets, instance pools for cost optimization, and how configuration affects pipeline reliability and performance.
---
## Key Concepts
**Cluster Types: New vs Existing vs Serverless**
New clusters are provisioned for each job run and torn down afterward. Good for isolation but slower startup. Existing clusters are pre running and reused. Faster but shared with other jobs. Serverless compute (Databricks managed) abstracts away cluster provisioning entirely, ideal for light workloads and quick prototyping.
**Spark Configuration and Tuning**
Set spark.sql.shuffle.partitions to control parallelism during shuffles (default 200). Lower for small data, higher for large. spark.sql.broadcastTimeout sets max size for broadcast joins. spark.executor.memory and spark.driver.memory control memory allocation. These are task level configs in databricks.yml.
**Library Management and Init Scripts**
Use pip to install Python packages, reference jars for JVM libraries, and egg files for custom Python code. Init scripts (bash scripts executed on cluster startup) enable custom environment setup: installing system dependencies, setting env vars, configuring credentials.
**Cluster Policies and Instance Pools**
Cluster policies enforce governance: only certain node types or DBU limits allowed. Instance pools pre warm compute for faster startup. These optimize cost and comply with organizational constraints without sacrificing developer flexibility.
---
## Code Examples
### Databricks Asset Bundle Configuration (databricks.yml)
```yaml
resources:
  jobs:
    etl_job:
      name: Daily ETL Pipeline
      tasks:
        - task_key: transform_data
          notebook_task:
            notebook_path: /notebooks/transform
          new_cluster:
            spark_version: "14.3.x-scala2.12"
            node_type_id: "m6i.xlarge"
            num_workers: 4
            spark_conf:
              spark.sql.shuffle.partitions: "512"
              spark.sql.broadcastTimeout: "300"
              spark.executor.memory: "8g"
            libraries:
              - pypi:
                  package: "pydantic==2.0.0"
              - pypi:
                  package: "requests==2.31.0"
            init_scripts:
              - destination: dbfs:/init_scripts/setup.sh
          timeout_seconds: 3600
          max_retries: 2
```
This DAB defines a job with a new cluster, Spark configs, PyPI libraries, and an init script. The timeout and max_retries provide reliability controls.
### Cluster Policy JSON
```json
{
  "spark_conf": {
    "spark.sql.shuffle.partitions": {
      "value": "512",
      "hidden": false
    },
    "spark.executor.memory": {
      "value": "8g",
      "hidden": true
    }
  },
  "node_type_id": {
    "type": "allowed_values",
    "values": ["m6i.xlarge", "m6i.2xlarge", "r6i.xlarge"]
  },
  "num_workers": {
    "type": "range",
    "minValue": 1,
    "maxValue": 16
  },
  "aws_attributes.availability": {
    "value": "ON_DEMAND",
    "hidden": false
  }
}
```
This policy restricts node types, sets min/max workers, and enforces on demand instances. Developers can provision clusters within these guardrails.
### Using Environment Variables and Init Scripts
```bash
# Init script (setup.sh) stored in dbfs:/init_scripts/setup.sh
#!/bin/bash
export DATABRICKS_TOKEN=$DATABRICKS_TOKEN
export API_KEY=$API_KEY
# Install system package
apt get install -y postgresql client
# Configure Spark with environment specific settings
echo "spark.driver.extraJavaOptions=-Dlog4j.configuration=file:///etc/spark/conf/log4j.properties" >> /etc/spark/conf/spark-defaults.conf
# Task in notebook accesses env vars
import os
api_key = os.environ.get("API_KEY")
token = os.environ.get("DATABRICKS_TOKEN")
# Set env var in DAB task
spark_conf:
  ENVIRONMENT: production
  LOG_LEVEL: INFO
```
Init scripts run at cluster startup to install dependencies and set environment. Task level spark_conf makes variables available to Spark.
---
## Common Exam Scenarios
**Scenario 1: Compliance driven cluster policy**
Your organization requires all clusters to use on demand instances for cost tracking and no more than 16 workers. Create a cluster policy that enforces these constraints. Developers provision clusters through the policy; any attempt to use spot instances or scale beyond 16 workers is rejected. This ensures governance without micromanaging individual job configs.
**Scenario 2: Environment specific configuration**
Your DAB targets both dev and prod. In dev, use small clusters (2 workers); in prod, use large clusters (8 workers). Use Databricks CLI targets to apply different configs per environment. Store API keys in environment variables set by init scripts so they are not hardcoded.
**Scenario 3: Serverless compute for fast feedback**
For a lightweight data quality check job that runs every hour, use serverless compute instead of provisioning a cluster. No warm up time, automatic scaling, and pay only for what you use. Ideal for short, bursty workloads.
---
## Key Takeaways
- New clusters provide isolation and custom config but slower startup. Existing clusters are faster but shared. Serverless is fastest for light workloads with no provisioning overhead.
- Databricks Asset Bundles (DABs) define job config as code in databricks.yml, making pipelines reproducible and version controlled
- Spark configs (shuffle.partitions, broadcastTimeout, executor.memory) tune performance. Set based on data size and cluster resources.
- Cluster policies enforce governance: restrict node types, DBU limits, and instance pools while keeping developers flexible
- Init scripts and environment variables manage secrets, install dependencies, and configure clusters without hardcoding in notebooks
---
## Gotchas and Tips
⚠️ Spark config scope matters: spark_conf at the task level overrides job level. Be careful with defaults to avoid surprises.
⚠️ Init scripts run as root: Be careful with permissions when setting environment variables. Log init script execution for debugging.
⚠️ Library installation time adds to startup: Heavy libraries can add minutes. Use instance pools to pre warm clusters if startup latency matters.
---
## Links and Resources
- **[Lakeflow Jobs](https://docs.databricks.com/aws/en/jobs/)** — Configuring job clusters, task dependencies, and environment settings.
- **[Databricks Asset Bundles](https://docs.databricks.com/aws/en/dev-tools/bundles)** — Defining job and environment configurations declaratively in YAML.