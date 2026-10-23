## Topic Overview
When you are building production data engineering projects on Databricks, how you structure your Python code matters a lot. **Databricks Asset Bundles (DABs)** give you a way to package your notebooks, libraries, pipelines, and job definitions into a single deployable unit. Think of DABs as your project's blueprint: they define what gets deployed, where it goes, and how it all connects together.
A well structured DAB project separates your source code, configuration files, tests, and deployment definitions into clear directories. This makes it easier to collaborate with your team, run CI/CD pipelines, and promote code across environments (dev, staging, production). The exam expects you to know what a typical DAB project looks like and how the pieces fit together.
DABs use a `databricks.yml` file at the project root as the main configuration file. This YAML file references your resources (jobs, pipelines, experiments) and defines how they should be deployed. You can also use variable substitution and target environments to manage differences between dev and prod setups without duplicating configuration.
---
## Key Concepts
**Standard DAB Project Layout**
A typical DAB project follows this structure: a root `databricks.yml` file for configuration, a `src/` directory for your Python modules and notebooks, a `resources/` directory for job and pipeline YAML definitions, a `tests/` directory for unit and integration tests, and optionally a `setup.py` or `pyproject.toml` for packaging your Python code as a wheel.
**The databricks.yml Configuration File**
This is the heart of your DAB project. It defines the bundle name, workspace targets (e.g., dev, staging, prod), and references to resource definition files. You can use variable substitution with `${var.environment}` syntax to parameterize deployments. The file also supports includes to pull in resource definitions from the `resources/` directory.
**Targets and Environment Management**
DABs support multiple deployment targets. Each target can override default values like the workspace URL, catalog, schema, and compute resources. This lets you use the same codebase for dev, test, and production without changing any source code. You deploy to a specific target using `databricks bundle deploy --target prod`.
**Python Wheel Builds in DABs**
For modular Python projects, you can package your code as a Python wheel and reference it in your DAB configuration. The bundle can automatically build the wheel during deployment using `artifacts` in `databricks.yml`. This is the recommended approach for sharing reusable code across multiple jobs and pipelines.
---
## Code Examples
### Minimal databricks.yml
```yaml
bundle:
  name: my_data_pipeline
include:
  - resources/*.yml
targets:
  dev:
    mode: development
    default: true
    workspace:
      host: https://dbc-abc123.cloud.databricks.com
  prod:
    mode: production
    workspace:
      host: https://dbc-xyz789.cloud.databricks.com
    run_as:
      service_principal_name: sp-data-pipeline
```
### Resource Definition (resources/pipeline_job.yml)
```yaml
resources:
  jobs:
    daily_etl:
      name: "Daily ETL Pipeline"
      schedule:
        quartz_cron_expression: "0 0 6 * * ?"
        timezone_id: "UTC"
      tasks:
        - task_key: ingest_data
          notebook_task:
            notebook_path: src/ingest.py
          job_cluster_key: etl_cluster
        - task_key: transform_data
          depends_on:
            - task_key: ingest_data
          notebook_task:
            notebook_path: src/transform.py
          job_cluster_key: etl_cluster
      job_clusters:
        - job_cluster_key: etl_cluster
          new_cluster:
            spark_version: "15.4.x-scala2.12"
            num_workers: 4
```
### Project Directory Structure
```
my_data_pipeline/
├── databricks.yml           # Main bundle configuration
├── resources/
│   ├── pipeline_job.yml     # Job definitions
│   └── dlt_pipeline.yml     # Lakeflow pipeline definitions
├── src/
│   ├── __init__.py
│   ├── ingest.py            # Ingestion notebook/script
│   ├── transform.py         # Transformation logic
│   └── utils/
│       ├── __init__.py
│       └── helpers.py       # Shared utility functions
├── tests/
│   ├── test_transform.py    # Unit tests
│   └── test_integration.py  # Integration tests
├── setup.py                 # For wheel packaging
└── requirements.txt         # Python dependencies
```
---
## Common Exam Scenarios
**Scenario 1: Choosing Where to Place Pipeline Definitions**
A team is setting up a new DAB project and needs to decide where to define their Lakeflow pipeline configuration. The correct approach is to place pipeline resource definitions in the `resources/` directory as separate YAML files and reference them from `databricks.yml` using the `include` directive. This keeps the main config clean and allows different team members to work on different resources without merge conflicts.
**Scenario 2: Deploying to Multiple Environments**
A data engineer needs to deploy the same pipeline to dev and prod with different compute configurations. The solution is to define multiple targets in `databricks.yml`, each with its own workspace URL, catalog, and cluster settings. Variable substitution using `${var.catalog_name}` lets you parameterize these differences. Deploy with `databricks bundle deploy --target prod`.
**Scenario 3: Packaging Shared Python Code**
Your team has utility functions used across multiple jobs. Rather than duplicating code in each notebook, the recommended approach is to package the shared code as a Python wheel using `setup.py` and configure the `artifacts` section in `databricks.yml` to build and upload the wheel automatically during deployment.
---
## Key Takeaways
- `databricks.yml` is the central configuration file for every DAB project. It defines the bundle name, targets, and references resource definitions.
- Keep resource definitions (jobs, pipelines) in a separate `resources/` directory and include them via the `include` directive.
- Use targets to manage multi environment deployments (dev, staging, prod) without duplicating code or config.
- Package shared Python code as wheels using `setup.py` or `pyproject.toml` and configure the `artifacts` section for automated builds.
- The CLI commands to know: `databricks bundle init`, `databricks bundle validate`, `databricks bundle deploy`, and `databricks bundle run`.
---
## Gotchas and Tips
Production targets should always use `mode: production` and set `run_as` to a service principal. Running production jobs as your personal user account is a common mistake that leads to failures when your credentials expire or your account is modified.
Do not put all your resource definitions directly in `databricks.yml`. This file can get unwieldy fast. Use the `include` pattern to split resources into smaller, focused files.
Always run `databricks bundle validate` before deploying. This catches configuration errors (wrong paths, invalid YAML, missing variables) before they reach your workspace.
---
## Links and Resources
- **[Databricks Asset Bundles](https://docs.databricks.com/aws/en/dev-tools/bundles)** — Overview of DABs for managing project deployments and CI/CD workflows.
- **[CI/CD with Databricks](https://docs.databricks.com/aws/en/dev-tools/ci-cd/)** — Best practices for continuous integration and deployment pipelines.