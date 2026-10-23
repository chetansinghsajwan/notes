A **data organization pattern** used in data lakehouses (popularized by Databricks) that structures data into **three progressive quality layers**.
---
### The Three Layers
|Layer|Also Called|Data Quality|Purpose|
|---|---|---|---|
|**Bronze**|Raw Layer|Raw, unfiltered|Ingest & store as-is|
|**Silver**|Refined Layer|Cleaned, validated|Filtered, joined, enriched|
|**Gold**|Curated Layer|Aggregated, trusted|Business-ready, analytics/ML|
---
### Key Points
- **Unidirectional flow** — data moves Bronze → Silver → Gold only
- **Incremental refinement** — each layer adds quality, not just copies data
- **Full history preserved** — Bronze retains original raw data always
- **Supports re-processing** — can recompute Silver/Gold from Bronze anytime
- **Schema enforcement** increases progressively per layer
---
### Common Tools Used
|Tool|Layer Typically Used|
|---|---|
|Apache Spark|All layers|
|Delta Lake / Iceberg|All layers|
|dbt|Silver → Gold|
|Apache Kafka|Bronze ingestion|
|Databricks|Full pipeline|
### ==Origin of Medallion Architecture==
**Databricks** coined the term. The first public mention was on **June 2, 2020**, in a Databricks blog post on monitoring audit logs that referenced _"our medallion architecture."_
- It was introduced as part of Databricks' **Lakehouse vision**, shortly after the release of **Delta Lake** (around 2019–2020).
- Since then, **Microsoft adopted it** for their central data storage solution, **OneLake**, part of the **Microsoft Fabric** platform.