System tables are a **Databricks-hosted analytical store** of your account's operational data located within the `system` catalog. They provide a centralized way to achieve **historical observability** across your entire Databricks account.
### Key Capabilities and Use Cases
You can use system tables to:
- **Monitor costs:** Track and analyze your account's billable usage and pricing over time.
- **Audit security events:** Review audit logs and track access patterns or network entry compliance.
- **Track compute and job performance:** Monitor configurations and lifecycles for clusters, SQL warehouses, and job/task run timelines.
- **Observe data and AI workloads:** Review table and column lineage, data quality monitoring checks, and MLflow or AI Gateway metrics.
### Important Characteristics
- **Location:** They reside under the `system` catalog and are organized into specific operational schemas like `access` and `billing`.
- **Read-Only:** System tables are read-only resources and cannot be altered or modified.
- **Account-Wide Coverage:** While they must be accessed through a Unity Catalog-enabled workspace, they securely aggregate operational data from **all workspaces** in your account within that region, including non-Unity Catalog workspaces.
- **Storage & Sharing:** The data is securely shared with you via **OpenSharing** and is hosted in a storage account located in the same region as your metastore. Most tables also feature a free retention period (frequently 365 days).
# References
- https://docs.databricks.com/aws/en/admin/system-tables