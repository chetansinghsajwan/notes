Difference between View, Temporary View and Materialized View:
**Quick comparison first:**
|Feature|View|Temporary View|Materialized View|
|---|---|---|---|
|Stores data physically|No|No|Yes|
|Persists after session|Yes|No|Yes|
|Registered in Unity Catalog|Yes|No|Yes|
|Query execution|Re-runs every time|Re-runs every time|Uses cached result|
|Query performance|Slower|Slower|Faster|
|Storage cost|None|None|Yes (cloud storage)|
|Refresh needed|No|No|Yes (manual or scheduled)|
|Created via|SQL / DataFrame|SQL / DataFrame|Databricks SQL or Lakeflow SDP|
---
**1. View**
- A view stores the text of a query against one or more data sources or tables in the metastore. Creating a view does not process or write any data — only the query text is registered.
- Pure **Apache Spark concept** — works in any Spark environment.
- In Databricks, views are registered in Unity Catalog and are queryable from anywhere in the workspace with proper permissions.
- Use when: you want to simplify/reuse complex SQL logic without storing data.
---
**2. Temporary View**
- Pure **Apache Spark concept** — exists in vanilla Spark too.
- A temporary view has limited scope and persistence and is not registered to a schema or catalog. In notebooks and jobs, temporary views are scoped to the notebook or script level — they cannot be referenced outside the notebook and no longer exist when the notebook detaches from the cluster. In Databricks SQL, temporary views are scoped to the query level.
- Use when: you need an intermediate dataset within a single notebook/script — like a CTE but reusable across cells.
---
**3. Materialized View**
- The concept exists in databases generally (PostgreSQL, Snowflake etc.), but **Databricks' implementation is unique**.
- Unlike standard views which recompute results on every query, materialized views cache the results and refresh them on a specified interval. A materialized view tracks changes in upstream data, incrementally processes changed data, and maintains the output table in sync with source data.
- In Databricks SQL, materialized views are Unity Catalog managed tables that physically store the results of a query. When you create one via Databricks SQL, a serverless pipeline is automatically created behind the scenes to handle refreshes.
- Two ways to create in Databricks: via **Databricks SQL** or via **Lakeflow SDP** (pipeline).
- Use when: you have expensive aggregations or joins that are queried frequently, especially for BI dashboards.
---
**Key Databricks-specific distinction:**
||Apache Spark|Databricks|
|---|---|---|
|View|Yes — native Spark|Same, registered in Unity Catalog|
|Temporary View|Yes — native Spark|Same, but scope differs (notebook vs query level)|
|Materialized View|Not natively in Spark|Databricks-specific feature, backed by Lakeflow SDP + Unity Catalog|