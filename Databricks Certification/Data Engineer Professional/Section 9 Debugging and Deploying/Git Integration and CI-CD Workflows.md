## Topic Overview
Modern data engineering requires integration with Git and CI/CD systems. Databricks Git Folders (formerly Repos) connect your workspace to a Git repository, enabling branch management, pull request workflows, and collaborative development. Combined with DABs and GitHub Actions or Azure DevOps, you can automate testing and promotion across environments.
The Professional exam tests your understanding of Git workflows in Databricks, how to structure CI/CD pipelines for data engineering, and best practices for code promotion and testing.
## Key Concepts
**Databricks Git Folders and Branch Management**
Git Folders connect a Databricks workspace to a Git repository. You can clone a repo, check out branches, and create new branches without leaving the UI. This enables multiple developers to work on different features in parallel. Commits are made from the Databricks UI or CLI, and changes sync back to the remote repository.
**Pull Request Workflows**
Create a feature branch, make changes, commit, and push to the remote. Open a PR for code review. Automated CI checks run on the PR (unit tests, linting, DAB validation). Once approved, merge to main. The main branch becomes the source of truth for production deployments. This workflow enforces quality gates before code reaches prod.
**CI/CD Integration with GitHub Actions and Azure DevOps**
GitHub Actions and Azure DevOps can automatically run tests, validate DABs, and deploy to Databricks when code is pushed or PRs are merged. Use the Databricks CLI in workflow files to run validation and deployment commands. This enables fully automated testing and promotion from dev through prod environments.
**Testing in CI Pipelines**
CI pipelines should run unit tests (using pytest on Spark with assertDataFrameEqual), validate PySpark code for syntax errors, and test Lakeflow pipelines in development mode. Fail fast: if unit tests fail, don't proceed to integration testing. Use code coverage tools to track test coverage and ensure critical paths are tested.
## Code Examples
### GitHub Actions Workflow for DAB Validation and Deployment
```yaml
# .github/workflows/deploy.yml
name: Deploy Data Pipeline
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
jobs:
  validate:
    runs on: ubuntu latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Install Databricks CLI
        run: pip install databricks-cli
      
      - name: Validate DAB
        env:
          DATABRICKS_HOST: ${{ secrets.DATABRICKS_HOST }}
          DATABRICKS_TOKEN: ${{ secrets.DATABRICKS_TOKEN }}
        run: |
          cd databricks_bundle
          databricks bundle validate --profile dev
      
      - name: Run Unit Tests
        run: |
          pip install pytest pyspark
          pytest tests/ -v
  deploy_to_prod:
    needs: validate
    if: github.ref == 'refs/heads/main' && github.event_name == 'push'
    runs on: ubuntu latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Install Databricks CLI
        run: pip install databricks-cli
      
      - name: Deploy to Production
        env:
          DATABRICKS_HOST: ${{ secrets.DATABRICKS_PROD_HOST }}
          DATABRICKS_TOKEN: ${{ secrets.DATABRICKS_PROD_TOKEN }}
        run: |
          cd databricks_bundle
          databricks bundle deploy --profile production
```
### Testing PySpark Code with assertDataFrameEqual
```python
import pytest
from pyspark.sql import SparkSession
from pyspark.testing import assertDataFrameEqual
from my_pipeline import process_customers
class TestCustomerProcessing:
    @pytest.fixture(scope="function")
    def spark(self):
        return SparkSession.builder.appName("test").getOrCreate()
    
    def test_process_customers(self, spark):
        # Arrange: Create test data
        input_data = [
            (1, "John", "john@example.com"),
            (2, "Jane", "jane@example.com")
        ]
        input_df = spark.createDataFrame(input_data, ["id", "name", "email"])
        
        # Act: Call the function
        result_df = process_customers(input_df)
        
        # Assert: Check the output
        expected_data = [
            (1, "JOHN", "john@example.com"),
            (2, "JANE", "jane@example.com")
        ]
        expected_df = spark.createDataFrame(expected_data, ["id", "name", "email"])
        
        assertDataFrameEqual(result_df, expected_df)
    
    def test_handles_nulls(self, spark):
        input_data = [(1, None, "email@example.com")]
        input_df = spark.createDataFrame(input_data, ["id", "name", "email"])
        
        result_df = process_customers(input_df)
        
        # Should handle null values gracefully
        assert result_df.count() == 1
```
## Common Exam Scenarios
**A developer creates a feature branch, makes changes to a notebook, and opens a PR. Your CI pipeline should automatically validate and test the changes. What checks would you include?**
Include: (1) DAB validation with bundle validate, (2) unit tests with pytest and assertDataFrameEqual, (3) linting (pylint or flake8 for Python), (4) code coverage checks, (5) DAB dry run to show what would be deployed. If any check fails, block the PR merge.
**You need to ensure that only code from the main branch gets deployed to production. How would you set this up with GitHub Actions and DABs?**
Create a GitHub Actions workflow that deploys only when the event is a push to main. Use conditional gates: if: github.ref == 'refs/heads/main' && github.event_name == 'push'. This ensures deployments only happen after PRs are merged into main and all checks pass.
## Key Takeaways
- Git Folders enable collaborative development with branch management and code review.
- CI/CD pipelines automate testing and validation before code reaches production.
- Always include DAB validation, unit tests, and linting in CI workflows.
- Main branch should be production ready; enforce strict merge requirements.
## Gotchas and Tips
Git Folder syncing is one way by default from the remote to the workspace. Commits go back to the remote, but changes from other users require a manual pull in the UI.
Secrets in CI/CD workflows should be managed as repository secrets, not hardcoded in workflow files.
---
## Links and Resources
- **[Git Folders (Repos)](https://docs.databricks.com/aws/en/repos/)** — Syncing notebooks and code with Git repositories for version control.
- **[CI/CD with Databricks](https://docs.databricks.com/aws/en/dev-tools/ci-cd/)** — Building automated CI/CD workflows with GitHub Actions, Azure DevOps, and Jenkins.