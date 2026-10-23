It allows you to describe Databricks resources such as jobs and pipelines as source files.
Bundles are defined using the file `databricks.yml` at the root of the project.
Formerly known as ==Databricks Assets Bundles==.
Your bundle project's collection of source files and metadata is deployed as a single bundle to your target environment. A bundle includes the following parts:
- Required cloud infrastructure and workspace configurations
- Source files, such as notebooks and Python files, that include the business logic
- Definitions and settings for Databricks resources, such as Lakeflow Jobs, Lakeflow Spark Declarative Pipelines, Dashboards, Model Serving endpoints, MLflow Experiments, and MLflow registered models
- Unit tests and integration tests
---
Declarative Automation Bundles are a feature of the Databricks CLI. You build bundles locally, then use the Databricks CLI to deploy your bundles to target remote Databricks workspaces and run bundle workflows in those workspaces from the command line.
# Auth
You deploy and run Declarative Automation Bundles within the context of two types of authentication scenarios: _attended_ and _unattended_:
- Attended authentication scenarios (user-to-machine) are manual workflows, for example, using your web browser on your local machine to log in to your target Databricks workspace when prompted by the Databricks CLI. This method is ideal for experimenting or for rapid development.
- Unattended authentication scenarios (machine-to-machine) are fully automated CI/CD workflows, for example, when using CI/CD systems such as GitHub.
---
Instance Pools
[[Variable Lookup]]
References

> [!info] What are Declarative Automation Bundles? | Databricks on AWS  
> Learn about Declarative Automation Bundles, which enable programmatic management of resources such as Lakeflow Jobs, Lakeflow Spark Declarative Pipelines, and MLOps stacks.  
> https://docs.databricks.com/aws/en/dev-tools/bundles/  

> [!info] Authentication for Declarative Automation Bundles | Databricks on AWS  
> Learn how to configure authentication for Declarative Automation Bundles.  
> https://docs.databricks.com/aws/en/dev-tools/bundles/authentication