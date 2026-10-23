A **dashboard** in Databricks is a visual interface that displays data as **charts, tables, and metrics** — built directly on top of your data in the Databricks platform.
---
**Two types of dashboards in Databricks:**
||Legacy Dashboard|AI/BI Dashboard (new)|
|---|---|---|
|Also called|DBSQL Dashboard|Lakeview Dashboard|
|Introduced|Early Databricks SQL|2024|
|Built with|Drag and drop widgets|Modern visual editor|
|AI powered|No|Yes — AI/BI Genie built in|
|Recommended|No — being deprecated|Yes — current standard|
|Sharing|Workspace only|Workspace + embedded|
---
**What you can put in a dashboard:**
- Bar, line, pie, scatter charts
- Tables
- Counters / KPI tiles
- Filters and dropdowns
- Text and markdown
- Maps
---
**How it works:**
```
Data in Delta Tables (Unity Catalog)
    → SQL Query (in Databricks SQL)
    → Visualization (chart / table)
    → Dashboard (collection of visualizations)
    → Shared with business users
```
---
**AI/BI Dashboard — what makes it special:**
- **AI/BI Genie** — business users can ask questions in plain English and get answers without writing SQL.
- Auto-generates charts from natural language queries.
- Governed by Unity Catalog — same permissions as underlying tables.
- Can be embedded in external apps.
---
**Databricks Dashboard vs other BI tools:**
|Feature|Databricks Dashboard|Power BI|Tableau|
|---|---|---|---|
|Lives inside Databricks|Yes|No|No|
|Needs data movement|No — queries live data|Often yes|Often yes|
|AI/natural language|Yes (Genie)|Copilot (limited)|Pulse AI|
|Governed by Unity Catalog|Yes|No|No|
|Best for|Data teams + governed BI|Business users|Visual analytics|
|Cost|Included in Databricks|Separate license|Separate license|
---
**In short:** A Databricks dashboard is a **governed, live BI layer** built directly on your lakehouse data — no data movement, no separate tool needed, with AI-powered querying built in.
# Source Control
==Status:== Source control for dashboards is currently in Public Preview — dashboards can now be integrated with Databricks Git folders for collaboration and version control. As of ==2026 April 20==.
---
==Two approaches:==
|Approach|When to use|
|---|---|
|Databricks Git Folders (native)|Recommended — direct integration|
|Export as file + manual Git|Fallback if Git folders not enabled|
---
==How it works — file format:==
Dashboards are exported as a JSON file with the format `.lvdash.json` — this file contains complete query syntax and widget settings.
---
==Approach 1 — Databricks Git Folders (recommended):==
- Place dashboards inside a Databricks Git folder to apply source control from the start.
- Git tracks all changes to the draft dashboard automatically.
- Commit, push, pull, branch — same as any code file.
==Approach 2 — Manual export + Git:==
- Export dashboard as `.lvdash.json`
- Add to your Git repo manually
- Edit in UI or directly in the JSON file
- Re-import via "Replace dashboard" when done
---
==CI/CD deployment with Databricks Asset Bundles:==
Store `.lvdash.json` files in Git repositories and deploy via `databricks bundle deploy`. Use variables like `${var.warehouse_id}` to parameterize across dev, staging, and production environments.
```yaml
# databricks.yml
resources:
  dashboards:
    sales_dashboard:
      display_name: 'Sales Dashboard'
      file_path: ./dashboards/sales_dashboard.lvdash.json
      warehouse_id: ${var.warehouse_id}
```
```bash
# Deploy to prod
databricks bundle deploy --target=prod
```
---
==Limitations to know:==
|Limitation|Detail|
|---|---|
|Max dashboards per Git folder|100|
|What Git does NOT track|Warehouse selection, publish config, schedules|
|Diff readability|Poor — JSON is long and hard to review in PRs|
|Git-based jobs|Not supported for dashboards|
|File format|Changes periodically as Databricks adds features|
---
==In short:== Dashboards are versioned as `.lvdash.json` files in Git — either natively via Databricks Git folders, or manually exported and managed. CI/CD deployment is done via Databricks Asset Bundles.
# References

> [!info] Dashboards | Databricks on AWS  
> Learn how to share insights with your team using AI/BI dashboards.  
> https://docs.databricks.com/aws/en/dashboards/  

> [!info] Version control dashboards with Git | Databricks on AWS  
> Learn how to use Databricks Git folders to version control AI/BI dashboards, collaborate with team members, and streamline deployment across workspaces.  
> https://docs.databricks.com/aws/en/dashboards/automate/git-support