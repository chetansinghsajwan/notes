## Topic Overview
Databricks Asset Bundles (DABs) represent a modern infrastructure as code approach for deploying and managing Databricks resources. Instead of manually creating jobs, pipelines, notebooks, and other resources through the UI, you define everything in YAML configuration files and deploy them programmatically. This shift transforms Databricks workspace management from a manual, error prone process into a repeatable, version controlled workflow.
Traditional deployment approaches rely on manual UI interactions or custom REST API/CLI scripts to create and manage resources. These methods lack version control for deployment configurations, making it difficult to reproduce environments across different stages (development, staging, production) and nearly impossible to implement proper CI/CD workflows. DABs solve these problems by providing a declarative, file based approach that integrates seamlessly with Git and standard DevOps tools.
A DAB project centers around a databricks.yml file that declares all resources you want to deploy. You use the Databricks CLI to validate, deploy, and run your resources. The targets feature allows you to parameterize configurations per environment, enabling the same DAB definition to work across development, staging, and production workspaces with different settings.
---
## Key Concepts
What are Databricks Asset Bundles (DABs)?
DABs are a declarative infrastructure as code framework for Databricks. You define your workspace resources (jobs, pipelines, notebooks, clusters, schemas) in YAML files and deploy them using the Databricks CLI. Each DAB is a directory containing a databricks.yml file and supporting resource files. DABs integrate with Git for version control and support environment based configurations through targets.
Traditional Deployment Issues
Manual UI based deployments lack version control, making it impossible to track who changed what and when. REST API/CLI scripts are custom one offs without standardization. Promoting environments (dev to staging to prod) requires manual duplication and adjustment of configurations. No built in parameterization means hardcoding values or maintaining separate scripts per environment. Reproducing production setups in new workspaces is error prone and time consuming.
DAB Workflow and Commands
A typical DAB workflow involves: (1) Define resources in databricks.yml and supporting files, (2) Run databricks bundle validate to check for syntax errors and missing resources, (3) Run databricks bundle deploy to create or update resources in the workspace, (4) Run databricks bundle run to trigger job or pipeline execution. Each command respects the target configuration, allowing the same DAB to deploy to different workspaces.
Targets and Environment Management
Targets allow you to define environment specific configurations within a single DAB. A target specifies the workspace URL, paths, variable values, and resource settings for a particular environment (dev, staging, prod). You specify which target to use when running bundle commands with the --target flag. This eliminates the need for separate DAB projects per environment and ensures consistency across deployments.
DABs vs REST API and CLI Scripts
REST API and custom CLI scripts offer fine grained control but require you to build and maintain the entire deployment logic. DABs provide a standardized, opinionated framework that handles validation, deployment, and targeting automatically. DABs are optimized for common use cases (deploying jobs, pipelines, notebooks) whereas REST APIs require more boilerplate. DABs integrate natively with Databricks CLI and Git based workflows, while custom scripts need additional integration work.
---
## Code Examples
### Basic databricks.yml Example
```yaml
bundle:
  name: my_data_pipeline
  version: "1.0.0"
resources:
  jobs:
    daily_ingest:
      name: Daily Data Ingestion
      tasks:
        - task_key: load_raw_data
          notebook_task:
            notebook_path: ./notebooks/ingest.py
          existing_cluster_id: cluster-12345
    
    daily_transform:
      name: Daily Data Transform
      tasks:
        - task_key: transform_data
          notebook_task:
            notebook_path: ./notebooks/transform.py
          existing_cluster_id: cluster-12345
      depends_on:
        - job_key: daily_ingest
  pipelines:
    production_pipeline:
      name: Production Data Pipeline
      target: "${var.catalog}.${var.schema}.gold"
      clusters:
        - label: default
          num_workers: 2
      libraries:
        - package: pyspark
      configuration:
        "spark.conf.spark.sql.shuffle.partitions": "200"
```
### Multi Target Configuration
```yaml
bundle:
  name: my_pipeline
variables:
  catalog:
    description: Unity Catalog name
    default: main
resources:
  jobs:
    process_data:
      name: Process Data
      tasks:
        - task_key: process
          notebook_task:
            notebook_path: ./notebooks/process.py
          existing_cluster_id: "${resources.clusters.compute.id}"
targets:
  dev:
    workspace:
      host: https://dev-workspace.cloud.databricks.com
      token: ${DBX_DEV_TOKEN}
    variables:
      catalog: dev_catalog
      cluster_id: dev-cluster-123
  
  staging:
    workspace:
      host: https://staging-workspace.cloud.databricks.com
      token: ${DBX_STAGING_TOKEN}
    variables:
      catalog: staging_catalog
      cluster_id: staging-cluster-456
  
  prod:
    workspace:
      host: https://prod-workspace.cloud.databricks.com
      token: ${DBX_PROD_TOKEN}
    variables:
      catalog: prod_catalog
      cluster_id: prod-cluster-789
```
### DAB Deployment Commands
```bash
# Validate the bundle configuration
databricks bundle validate
# Validate for a specific target
databricks bundle validate --target dev
# Deploy to development
databricks bundle deploy --target dev
# Deploy to staging
databricks bundle deploy --target staging
# Deploy to production
databricks bundle deploy --target prod
# Run a specific job in the dev environment
databricks bundle run daily_ingest --target dev
# View the plan of what will be deployed (dry run)
databricks bundle deploy --target prod --dry-run
# List all resources in the bundle
databricks bundle list
```
### Traditional REST API Comparison
```bash
# Traditional approach: manual REST API calls
# This requires custom scripting and error handling
# Create a job using REST API
curl -X POST https://your-workspace.databricks.com/api/2.1/jobs/create \
  -H "Authorization: Bearer <token>" \
  -d '{
    "name": "Daily Ingest",
    "tasks": [
      {
        "task_key": "load_data",
        "notebook_task": {
          "notebook_path": "/Users/user/notebooks/ingest"
        },
        "existing_cluster_id": "cluster-123"
      }
    ]
  }'
# Update the same job for staging (manual duplication with different IDs)
curl -X POST https://staging-workspace.databricks.com/api/2.1/jobs/create \
  -H "Authorization: Bearer <staging-token>" \
  -d '{
    "name": "Daily Ingest",
    "tasks": [
      {
        "task_key": "load_data",
        "notebook_task": {
          "notebook_path": "/Users/user/notebooks/ingest"
        },
        "existing_cluster_id": "cluster-456"
      }
    ]
  }'
# With DABs, a single command handles both:
databricks bundle deploy --target dev
databricks bundle deploy --target staging
```
---
## Common Exam Scenarios
### Scenario 1: Deploying the Same Pipeline to Multiple Environments
A team needs to deploy a data pipeline to development, staging, and production workspaces with different cluster sizes and catalog names. Using a traditional approach, they would need to create three separate job definitions with hardcoded workspace URLs and cluster IDs. With DABs, they define the pipeline once in databricks.yml and use targets to specify environment specific values. A single databricks bundle deploy --target prod command deploys to production with the correct workspace URL, cluster size, and catalog name. This eliminates copy paste errors and ensures consistency across all environments.
### Scenario 2: Version Controlling Deployment Configurations
A team manually creates jobs through the Databricks UI and loses track of what changed and when. An engineer modifies a job, causing a production outage, but no one can see who made the change or revert it. With DABs, the databricks.yml file lives in Git, so every change has a commit message, author, and timestamp. The team can see exactly what changed in a pull request before deployment. If a change causes issues, they can quickly revert to the previous version. This provides audit trails and enables code review for infrastructure changes.
### Scenario 3: Implementing CI/CD for Data Pipelines
A team wants to run automated tests and deploy pipelines through a CI/CD system (GitHub Actions, GitLab CI). Traditional approaches require custom scripting to call REST APIs. DABs integrate seamlessly with CI/CD: the pipeline runs databricks bundle validate to check configurations, then databricks bundle deploy --target staging to deploy to staging for testing, and finally databricks bundle deploy --target prod upon approval. The same DAB definition is used throughout the entire workflow, reducing opportunities for configuration drift.
---
## Key Takeaways
- DABs provide infrastructure as code for Databricks, replacing manual UI interactions with declarative YAML configuration files that integrate with Git and standard DevOps tools.
- Key commands are: databricks bundle validate (check config), databricks bundle deploy (create/update resources), and databricks bundle run (execute jobs/pipelines).
- Targets enable environment based parameterization, allowing a single DAB to deploy to dev, staging, and prod with different configurations.
- DABs solve traditional deployment issues: lack of version control, manual duplication, configuration drift, and difficulty promoting environments.
- DABs support deploying jobs, pipelines, notebooks, clusters, schemas, and other Databricks resources with a single, standardized framework.
---
## Gotchas and Tips
Secrets and Environment Variables
Never hardcode tokens, passwords, or API keys directly in databricks.yml. Use environment variables (referenced with ${VARIABLE_NAME}) or Databricks secrets. Store sensitive values in CI/CD system secrets or local environment configuration files that are .gitignored.
Path References in Resources
When referencing notebook paths in databricks.yml, use relative paths (e.g., ./notebooks/my_notebook.py) from the bundle root directory. The Databricks CLI automatically resolves these relative paths and uploads the notebooks to the workspace during deployment. Absolute paths or workspace URLs can cause path resolution issues.
Dry Run Before Production Deployment
Always run databricks bundle deploy --target prod --dry-run before actually deploying to production. This shows you exactly what resources will be created, updated, or deleted without actually making changes. It catches configuration errors and unintended side effects before they impact production.
---
## Links and Resources
- **[What are Databricks Asset Bundles?](https://docs.databricks.com/aws/en/dev-tools/bundles)** — Overview of DABs for infrastructure as code deployments.
- **[Databricks Asset Bundle deployment modes](https://docs.databricks.com/aws/en/dev-tools/bundles/deployment-modes)** — Development vs production deployment mode differences.
- **[CI/CD on Databricks](https://docs.databricks.com/aws/en/dev-tools/ci-cd/)** — Overview of CI/CD approaches including DABs and Git integration.