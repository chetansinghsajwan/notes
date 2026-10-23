## Topic Overview
**Databricks Notebooks** are the primary interactive development environment within the Databricks platform. They support Python, SQL, Scala, and R, and you can mix languages within a single notebook using magic commands. Notebooks are where most data engineers write, test, and iterate on their code before productionizing it.
Beyond just running code, notebooks in Databricks come with built in features like version history, collaboration, widgets for parameterization, and integration with Lakeflow Jobs for scheduling. They also have a results pane that renders tables, charts, and visualizations directly below each cell.
For the exam, you should know the key capabilities of notebooks, including magic commands, how notebooks attach to clusters, how to parameterize them with widgets, and how they integrate with Git (Databricks Git Folders, formerly Repos).
---
## Key Concepts
**Magic commands**
Magic commands let you switch languages or perform special operations within a notebook cell. The most common ones are: `%python`, `%sql`, `%scala`, `%r` for language switching. `%md` renders Markdown. `%run` executes another notebook in the same context (sharing variables). `%fs` gives you access to DBFS commands. `%sh` runs shell commands on the driver node.
**Widgets for parameterization**
Databricks widgets let you add input parameters to notebooks. There are four types: `text` (free text input), `dropdown` (select from a list), `combobox` (free text with suggestions), and `multiselect` (choose multiple values). You create them with `dbutils.widgets` and retrieve values with `dbutils.widgets.get()`. Widgets are essential for making notebooks reusable across different environments or datasets.
**Notebook workflows with %run**
The `%run` magic command executes another notebook inline, as if its code were part of the calling notebook. This means all variables, functions, and imports from the called notebook become available in the caller. This is commonly used for shared utility functions or configuration setup. Important: `%run` must be in its own cell and cannot be mixed with other code. For more complex workflows with separate execution contexts, use `dbutils.notebook.run()` instead.
**Databricks Git Folders (formerly Repos)**
Databricks Git Folders let you sync notebooks and files with a remote Git repository (GitHub, GitLab, Azure DevOps, Bitbucket). You can clone repos, create branches, commit, push, and pull directly from the Databricks UI. This enables proper version control and collaboration workflows. Notebooks in Git Folders are stored as source files (not the Databricks proprietary format), making them compatible with standard code review tools.
**Cluster attachment and execution context**
Every notebook must be attached to a cluster (or serverless compute) to execute code. When you attach a notebook, an execution context is created on the cluster for that notebook. Variables and state persist across cells within that context. Detaching and reattaching clears the state. Multiple notebooks can share the same cluster but have isolated execution contexts.
---
## Code Examples
### Using widgets to parameterize a notebook
```python
# Create widgets
dbutils.widgets.text("env", "dev", "Environment")
dbutils.widgets.dropdown("region", "us-east-1", ["us-east-1", "us-west-2", "eu-west-1"], "Region")
# Retrieve widget values
env = dbutils.widgets.get("env")
region = dbutils.widgets.get("region")
print(f"Running in {env} environment, region: {region}")
# Use in queries
catalog = f"{env}_catalog"
df = spark.read.table(f"{catalog}.sales.transactions")
# Remove widgets when done
dbutils.widgets.removeAll()
```
### Running another notebook with %run vs dbutils.notebook.run()
```python
# %run executes in the SAME context (shares variables)
# Must be the only content in the cell
%run ./includes/setup
# After %run, variables from 'setup' notebook are available here
print(config_variable)  # defined in the setup notebook
```
```python
# dbutils.notebook.run() executes in a SEPARATE context
# Returns a string result, does NOT share variables
result = dbutils.notebook.run(
    "./transforms/process_orders",
    timeout_seconds=600,
    arguments={"date": "2025-01-15", "env": "prod"}
)
print(f"Notebook returned: {result}")
```
### Accessing widgets in SQL
```sql
-- Create a widget in SQL
CREATE WIDGET TEXT env DEFAULT 'dev';
-- Use the widget value with getArgument()
SELECT * FROM identifier(getArgument('env') || '_catalog.sales.orders')
WHERE order_date = current_date();
```
---
## Common Exam Scenarios
**Scenario 1: Sharing code between notebooks**
A data engineer has utility functions used across multiple notebooks. They want to call these functions without duplicating code. The correct approach is to put the utility functions in a separate notebook and use `%run ./utils` at the top of each consuming notebook. This executes the utility notebook in the same context, making all functions available. If the engineer needs to pass parameters and get a result back, `dbutils.notebook.run()` is the right choice instead.
**Scenario 2: Parameterizing a notebook for different environments**
A notebook needs to run against both a dev and prod catalog depending on the deployment. The engineer should use widgets (dbutils.widgets) to accept an environment parameter, then dynamically construct table references. When the notebook is scheduled via Lakeflow Jobs, the job definition can pass widget values as task parameters.
**Scenario 3: Mixed language usage**
A notebook's default language is Python, but the engineer wants to run a SQL query in one cell. They simply add `%sql` at the top of that cell. To pass data between languages, you can use temporary views. Create a temp view in SQL, then read it in Python with `spark.table("temp_view_name")`.
---
## Key Takeaways
- Notebooks support Python, SQL, Scala, and R with language switching via magic commands (%python, %sql, etc.).
- %run executes another notebook in the same context (shares variables). dbutils.notebook.run() executes in a separate context and returns a string.
- Widgets (text, dropdown, combobox, multiselect) parameterize notebooks. Values are retrieved with dbutils.widgets.get().
- Databricks Git Folders (formerly Repos) enable version control with remote Git providers.
- Each notebook gets its own execution context on the attached cluster. Variables persist across cells but are lost when detached.
---
## Gotchas and Tips
**%run must be alone in a cell.** You cannot combine %run with other code in the same cell. It must be the only statement. If you need to run a notebook and then do something with the result, use dbutils.notebook.run() instead.
**%run vs dbutils.notebook.run() is a common exam trap.** Remember: %run = same context, shares variables, no return value. dbutils.notebook.run() = separate context, returns a string, accepts parameters. If a question asks about sharing variables between notebooks, the answer is %run.
**Temp views bridge languages.** When you need to pass data between a SQL cell and a Python cell, create a temporary view in SQL (CREATE OR REPLACE TEMP VIEW), then access it in Python with spark.table(). This is the standard pattern for cross language data sharing in notebooks.
---
## Links and Resources
- **[Databricks notebooks](https://docs.databricks.com/aws/en/notebooks/)** — Complete guide to creating, running, and managing notebooks in Databricks.
- **[Manage notebooks](https://docs.databricks.com/aws/en/notebooks/notebooks-manage)** — How to create, open, delete, rename, and control access to notebooks.