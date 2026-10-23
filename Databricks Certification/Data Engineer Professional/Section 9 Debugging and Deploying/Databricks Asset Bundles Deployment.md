## Topic Overview
Databricks Asset Bundles (DABs) are the modern way to define, test, and deploy data engineering workflows as code. Instead of clicking through the UI to create jobs, notebooks, and pipelines, you define them in YAML, commit them to Git, and deploy across environments. This approach brings infrastructure as code practices to Databricks, enabling version control, code review, and continuous deployment.
The Professional exam tests your ability to structure DAB projects, understand the deployment lifecycle, and integrate DABs with CI/CD pipelines. You need to know the difference between development and production deployment modes, how to manage secrets, and how to promote code across workspaces.
## Key Concepts
**Bundle Lifecycle: Init, Validate, Deploy, Destroy**
bundle init scaffolds a new DAB project. bundle validate checks syntax and references. bundle deploy pushes resources to a Databricks workspace. bundle destroy removes deployed resources. This lifecycle mirrors infrastructure management tools like Terraform.
**Development vs. Production Deployment Modes**
Development mode deploys to a dev workspace with shorter job timeouts and lenient defaults, ideal for testing. Production mode deploys to a prod workspace with strict resource controls and enforcement. The same bundle.yml can target both environments by using profiles. Profiles allow environment specific variable overrides without changing the core bundle definition.
**Artifact Management and Dependencies**
DABs support artifact references: notebooks, JAR files, Python wheels, and more. You can reference artifacts by path, and the bundle system resolves dependencies. Libraries are packaged and deployed alongside jobs. This ensures reproducible deployments where code and dependencies are versioned together.
**Multi Workspace Promotion Strategy**
Deploy the same bundle to dev, staging, and prod workspaces using profiles and workspace parameters. Each workspace has its own resource IDs but runs the same logic. This enables code promotion: test in dev, promote through staging, and finally to prod. Use CI/CD pipelines to automate validation and promotion across workspaces.
## Code Examples
### Basic DAB Structure and bundle.yml
```yaml
# bundle.yml - Root configuration
name: my-data-pipeline
bundle:
  environment: dev  # Can be overridden by profile
resources:
  jobs:
    daily_etl:
      name: Daily ETL Job
      job_cluster_config:
        spark_version: "13.3.x-scala2.12"
        node_type_id: "i3.xlarge"
        num_workers: 2
      tasks:
        - task_key: ingest
          notebook_task:
            notebook_path: ./notebooks/ingest
          libraries:
            - pypi:
                package: pydantic==1.0.0
        - task_key: transform
          depends_on:
            - task_key: ingest
          sql_task:
            file: ./sql/transform.sql
  pipelines:
    silver_pipeline:
      name: Silver Data Pipeline
      storage: /Volumes/my_catalog/pipelines/silver
      clusters:
        - label: default
          num_workers: 2
profiles:
  development:
    bundle:
      environment: dev
    resources:
      jobs:
        daily_etl:
          max_concurrent_runs: 1
  production:
    bundle:
      environment: prod
    resources:
      jobs:
        daily_etl:
          max_concurrent_runs: 5
          timeout_seconds: 7200
```
### DAB CLI Commands and Deployment
```bash
# Initialize a new bundle
databricks bundle init
# Validate the bundle without deploying
databricks bundle validate
# Deploy to development workspace (default profile)
databricks bundle deploy
# Deploy to production using profile
databricks bundle deploy --profile production
# Show what will be deployed (dry run)
databricks bundle deploy --dry-run
# Destroy resources (removes from workspace)
databricks bundle destroy
# List deployed resources
databricks bundle summary
```
### Parameterization and Variable Substitution
```yaml
variables:
  cluster_size:
    default: 2
  notebook_path:
    default: ./notebooks/process
  job_schedule:
    default: "0 2 * * *"  # 2 AM daily
resources:
  jobs:
    process_job:
      name: Processing Job
      schedule:
        quartz_cron_expression: ${var.job_schedule}
        timezone_id: UTC
      tasks:
        - task_key: main
          notebook_task:
            notebook_path: ${var.notebook_path}
          new_cluster:
            spark_version: 13.3.x-scala2.12
            node_type_id: i3.xlarge
            num_workers: ${var.cluster_size}
# Override variables in profile
profiles:
  production:
    variables:
      cluster_size: 8
      job_schedule: "0 1 * * *"  # 1 AM for prod
```
## Common Exam Scenarios
**You want to deploy the same pipeline code to dev and prod workspaces but with different cluster sizes. The dev cluster should have 1 worker, prod should have 4. How would you structure this in DAB?**
Use profiles with variable overrides. Define a variable cluster_size in the main bundle.yml. Create a development profile and a production profile, each overriding the variable with the appropriate value. Deploy with bundle deploy --profile development or bundle deploy --profile production.
**After deploying a job with DAB, someone makes a manual change in the UI (edits the job schedule). The next DAB deploy will overwrite this change. How do you prevent accidental overwrites?**
Always use bundle deploy --dry-run before applying changes to see what will change. Enforce code review: require PRs before deploying. Use CI/CD pipelines to deploy only from approved commits. Document the rule that DAB is the source of truth and manual UI changes should not be made.
## Key Takeaways
- DABs bring infrastructure as code to Databricks, enabling version control and code review.
- Use profiles to deploy the same bundle to multiple environments with different configurations.
- Validate before deploying with bundle validate to catch errors early.
- DAB is the source of truth; avoid making manual changes in the UI.
## Gotchas and Tips
bundle deploy will overwrite UI changes. If someone manually edits a job after deployment, the next deploy will revert to the bundle configuration.
Variables in DAB are only substituted during deployment. Changes to variable values in a profile require a redeploy to take effect.
---
## Links and Resources
- **[Databricks Asset Bundles](https://docs.databricks.com/aws/en/dev-tools/bundles)** — Deploying jobs, pipelines, and notebooks using DABs with environment-specific overrides.
- **[CI/CD with Databricks](https://docs.databricks.com/aws/en/dev-tools/ci-cd/)** — Integrating DAB deployments into CI/CD pipelines.