[[Course Notes]]
Lakeflow Spark Declarative Pipelines (SDP) is a framework for creating batch and streaming data pipelines in SQL and Python. Lakeflow SDP extends and is interoperable with Apache Spark Declarative Pipelines, while running on the performance-optimized Databricks Runtime.
**In simple terms:**  
Lakeflow SDP = Apache Spark SDP + Databricks enterprise features on top.
**What it adds over Apache SDP:**
|**Addition**|**What it means**|
|---|---|
|**Auto CDC**|Handles out-of-order CDC events automatically|
|**SCD Type 1 and 2**|No manual merge logic needed|
|**Unity Catalog integration**|Governance, lineage, permissions built-in|
|**Pipeline DAG UI**|Visual view of your pipeline in Databricks IDE|
|**Advanced data quality**|Expectations stored and versioned in Unity Catalog|
|**Optimized runtime**|Up to 5x better price/performance vs standard Spark|
|**Observability**|Built-in monitoring, update history, lineage tracking|
**Origin in one line:**  
Lakeflow SDP is the **rebrand of Delta Live Tables (DLT)** — same product, evolved and renamed after Databricks contributed the core to Apache Spark in 2025.
**For you specifically:**  
When you build pipelines on Azure Databricks, you are using Lakeflow SDP — and features like automatic SCD Type 2 coalescing of duplicate records with the same natural key are Lakeflow-only features not available in open-source Apache SDP.