## Topic Overview
A Databricks Asset Bundle (DAB) is a declarative framework for packaging, deploying, and managing Databricks resources as code. The bundle follows a strict directory and configuration structure that defines how your code, jobs, pipelines, and other assets are organized locally and then synced and deployed to a Databricks workspace.
The foundation of every DAB is the databricks.yml configuration file. This YAML file sits at the root of your bundle and contains the bundle metadata, resource definitions, variables, environment targets, and workspace configuration. Everything else your notebooks, Python scripts, test files, and library packages lives in subdirectories that are referenced by databricks.yml.
When you run databricks bundle deploy, the DAB CLI reads your configuration, builds any artifacts (like wheel files), syncs your local files to the workspace, and creates or updates all the resources defined in your bundle. This makes it easy to version control your entire data infrastructure and reproduce deployments consistently across environments.
---
## Key Concepts
databricks.yml Anatomy
The databricks.yml file has these main sections:
- bundle: Metadata like name and version
- variables: Parameterized values used throughout the config
- resources: Job definitions, Lakeflow pipelines, schemas, models, experiments
- targets: Environment specific overrides (dev, staging, prod)
- workspace: Configuration for syncing to the remote workspace (host, auth, path)
- artifacts: Build instructions for libraries (e.g., Python wheels)
Resources Block
The resources: section defines all the Databricks assets that will be created or updated on deploy. Common resource types include:
- jobs: Scheduled or triggered workflows (tasks, schedule, clusters, parameters)
- pipelines: Lakeflow Spark Declarative Pipelines (batch or streaming ingestion and transformation)
- schemas: Unity Catalog schemas
- models: Unity Catalog registered models
- experiments: MLflow experiments for tracking ML runs
Each resource is a named block containing configuration properties. Notebooks referenced in job tasks use relative paths from the bundle root, making it easy to move the entire bundle between workspaces.
Variables and Parameterization
Variables allow you to parameterize your bundle configuration. Define them in the variables: section with default values, then reference them in resources using ${var.variable_name}. Variables are particularly useful for:
- Cluster sizing and configuration (per environment)
- Data paths and schema names
- Timeout and retry values for jobs
- Environment specific settings (dev uses cheaper clusters, prod uses larger ones)
The targets: section lets you override variable values per environment. When deploying, use databricks bundle deploy --target=prod to select which target's overrides apply.
Include for Modular Configs
For larger bundles, the include: directive lets you split configuration across multiple YAML files instead of one monolithic databricks.yml. This makes bundles easier to organize and maintain.
Example: You could have resources/jobs.yml, resources/pipelines.yml, and resources/schemas.yml each with their own resource definitions. The main databricks.yml then includes all of them:
```yaml
include:
  - resources/jobs.yml
  - resources/pipelines.yml
  - resources/schemas.yml
```
Artifacts for Libraries
The artifacts: section defines how to build libraries (typically Python wheels) from source code in your bundle. The build process runs before deployment.
A typical artifact entry specifies:
- Type: usually whl for Python wheel
- Path: source directory where setup.py lives
- Executable: path to the build script (often python setup.py bdist_wheel)
Jobs can then reference and install the built wheel, allowing you to package custom Python code and deploy it with your bundle.
Workspace and File Sync
The workspace: section in your bundle configuration tells the DAB CLI how to connect to and sync files to the Databricks workspace. It typically includes:
- host: The Databricks workspace URL (e.g., https://my-workspace.cloud.databricks.com)
- token: Authentication token or a reference to a profile
- root_path: The remote workspace path where files are synced (e.g., /Workspace/Users/user@company.com/my-bundle)
When you deploy, the DAB CLI syncs all your notebooks, Python files, and other source code to the workspace root path. This allows job tasks to reference notebooks by relative paths.
---
## Code Examples
Complete databricks.yml with main sections:
```yaml
bundle:
  name: my-data-pipeline
  version: 1.0.0
variables:
  environment:
    default: dev
  num_workers:
    default: 2
  catalog_name:
    default: main
  schema_name:
    default: raw_data
  batch_size:
    default: 1000
resources:
  jobs:
    ingest_data:
      name: "Daily Data Ingestion - ${var.environment}"
      tasks:
        - task_key: ingest
          notebook_task:
            notebook_path: ./src/ingest
            base_parameters:
              batch_size: "${var.batch_size}"
          new_cluster:
            spark_version: "13.3.x-scala2.12"
            node_type_id: "i3.xlarge"
            num_workers: "${var.num_workers}"
          timeout_seconds: 3600
    transform_data:
      name: "Transform Data - ${var.environment}"
      tasks:
        - task_key: transform
          notebook_task:
            notebook_path: ./src/transform
          depends_on:
            - task_key: ingest
          new_cluster:
            spark_version: "13.3.x-scala2.12"
            num_workers: "${var.num_workers}"
```
Multi task job with dependencies:
```yaml
resources:
  jobs:
    multi_task_job:
      name: "Multi-Task ETL Job"
      tasks:
        - task_key: extract
          notebook_task:
            notebook_path: ./src/extract
            base_parameters:
              source_table: customer_raw
          new_cluster:
            spark_version: "13.3.x-scala2.12"
            node_type_id: "i3.xlarge"
            num_workers: 2
          timeout_seconds: 1800
        - task_key: transform
          depends_on:
            - task_key: extract
          notebook_task:
            notebook_path: ./src/transform
          new_cluster:
            spark_version: "13.3.x-scala2.12"
            num_workers: 2
        - task_key: load
          depends_on:
            - task_key: transform
          notebook_task:
            notebook_path: ./src/load
        - task_key: quality_check
          depends_on:
            - task_key: transform
          notebook_task:
            notebook_path: ./src/data_quality
```
Variables with environment specific targets:
```yaml
variables:
  cluster_size: small
  num_workers: 2
  instance_type: i3.xlarge
  notification_email: dev-team@company.com
targets:
  dev:
    variables:
      cluster_size: small
      num_workers: 2
      instance_type: i3.xlarge
      notification_email: dev-team@company.com
  staging:
    variables:
      cluster_size: medium
      num_workers: 4
      instance_type: i3.2xlarge
      notification_email: staging-team@company.com
  prod:
    variables:
      cluster_size: large
      num_workers: 8
      instance_type: i3.4xlarge
      notification_email: ops-team@company.com
```
Include pattern with modular configuration:
```yaml
bundle:
  name: modular-bundle
  version: 1.0.0
include:
  - ./resources/variables.yml
  - ./resources/jobs.yml
  - ./resources/pipelines.yml
  - ./resources/schemas.yml
  - ./targets.yml
workspace:
  host: "https://my-workspace.cloud.databricks.com"
  token: "${env.DATABRICKS_TOKEN}"
  root_path: "/Workspace/Users/user@company.com/modular-bundle"
```
Typical bundle directory structure:
```bash
my-data-bundle/
├── databricks.yml              # Root config file
├── src/
│   ├── ingest.py
│   ├── transform.py
│   ├── load.py
│   └── data_quality.py
├── tests/
│   ├── test_ingest.py
│   └── test_transform.py
├── resources/
│   ├── jobs.yml
│   ├── pipelines.yml
│   └── variables.yml
├── .databricks/
│   └── state/
└── my_package/
    ├── setup.py
    └── my_package/
```
---
## Common Exam Scenarios
Scenario 1: Environment specific configuration
A company has a data pipeline defined in a single DAB. In development, they use small clusters with 2 workers to save costs. In production, they need large clusters with 8 workers for performance. How would you structure the databricks.yml to support this?
Answer: Use the targets: section in databricks.yml with variable overrides. Define a variable num_workers with default value 2 (dev), then in the targets.prod section, override it to 8. Reference ${var.num_workers} in the cluster configuration. Deploy with databricks bundle deploy --target=prod to use the prod overrides.
Scenario 2: Resource dependencies and multi task jobs
Your bundle defines a job with four tasks: extract, transform, load, and quality check. The extract task should run first, then transform depends on extract, and both load and quality check depend on transform. How do you structure this in the resources section?
Answer: Each task has a task_key and an optional depends_on block. The extract task has no depends_on (runs immediately). Transform has depends_on: [task_key: extract]. Both load and quality check have depends_on: [task_key: transform]. This creates a DAG where extract runs first, then transform, then load and quality check run in parallel.
Scenario 3: Modularizing a large bundle
Your databricks.yml file has grown very large with 20+ jobs, 15 pipelines, and complex variable definitions. It's becoming hard to maintain. How would you restructure it?
Answer: Use the include: directive to split configuration into separate files. Create resources/jobs.yml and resources/pipelines.yml for resource definitions, and variables.yml for variable definitions. In the main databricks.yml, include all of them. Each file is now smaller and easier to manage. The CLI merges them at runtime.
---
## Key Takeaways
- The DAB is rooted in databricks.yml which defines all bundle metadata, resources, variables, and workspace configuration.
- Variables enable parameterization; targets: section allows environment specific overrides.
- Resources define jobs (with task dependencies), pipelines, schemas, models, and experiments.
- Use include: to split large bundles into modular YAML files for better maintainability.
- File sync ensures your local source code is always reflected in the workspace at the configured root path.
---
## Gotchas and Tips
Variable interpolation happens at deploy time. If you reference a variable in databricks.yml but don't define it as a variable, the deployment will fail. Always declare variables you plan to use.
Notebook paths in job task definitions are relative to the bundle root on disk. Ensure your notebook_path points to a valid file. For example, ./src/ingest looks for a file named ingest.py in the src/ directory.
When using include: to split configs, file paths are relative to the bundle root, not relative to the including file. Always use paths from the bundle root.
---
## Links and Resources
- **[Databricks Asset Bundle configuration](https://docs.databricks.com/aws/en/dev-tools/bundles/settings)** — How to structure databricks.yml and configure bundle settings.
- **[Configuration reference](https://docs.databricks.com/aws/en/dev-tools/bundles/reference)** — Full reference for all bundle configuration options.
- **[Databricks Asset Bundles resources](https://docs.databricks.com/aws/en/dev-tools/bundles/resources)** — Defining jobs, pipelines, and other resources in bundle configs.