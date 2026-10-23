In **Databricks Asset Bundles (DABs)**—now referred to as **Declarative Automation Bundles**—a **variable lookup** is a powerful feature that allows your bundle configuration to dynamically retrieve the unique identifier (ID) of an existing Databricks resource based on its name.
Instead of hardcoding environment-specific IDs (such as a specific SQL Warehouse ID or Cluster ID) directly into your `databricks.yml` file, you can instruct the bundle to look up that resource in the target workspace at deployment time.
### Why Use Variable Lookups?
- **Multi-Environment Deployment:** It allows the exact same bundle configuration to deploy seamlessly across development, staging, and production workspaces without modification.
- **Avoids Hardcoding:** Eliminates the need to copy-paste unique IDs or use custom wrapper scripts to discover resources dynamically.
- **Integrates with Shared Infrastructure:** If your DevOps team provisions a shared SQL warehouse across environments using Terraform, your bundle can automatically discover it by its given name.
### How it Works (Syntax Example)
To use a variable lookup, you define a custom variable in your configuration file using the `lookup` mapping.
YAML
```
variables:
  my_warehouse_id:
    description: "The ID of the shared SQL warehouse"
    lookup:
      warehouse: "Data processing warehouse endpoint"
```
In this example:
1. You declare a custom variable named `my_warehouse_id`.
2. The `lookup` mapping tells the Databricks CLI to search the target workspace for a **SQL warehouse** named `"Data processing warehouse endpoint"`.
3. When you run `databricks bundle deploy`, the CLI queries the workspace, retrieves that specific warehouse's ID, and maps it to your variable.
4. You can then reference it elsewhere in your bundle resources using standard substitution syntax: `${var.my_warehouse_id}`.
### Supported Lookup Types
Variable lookups support several Databricks workspace resource types, including:
- `warehouse` (SQL Warehouses)
- `cluster` (Existing Compute Clusters)
- `notification_destination` (Alert and notification endpoints)