  
Questions:
- What is the difference between streaming table and materialized view?
---
==Lakeflow connect -== A set of ingestion connectors to ingest data from different kinds of sources. Sources like CRMs, database and cloud object storage.
==Lakeflow Spark Declarative Pipelines (SDP) -== A framework for building batch or streaming data pipelines by declaring the configuration and leaving the implementation handling to databricks. The goal is to accelerate ETL development.
==Lakeflow Jobs -== Didn’t understood
## Create Spark Declarative Pipelines
![[image 5.png]]
==Streaming Table -== A table with support for streaming or incremental data processing. It only processes new data.
==View -== A virtual table with no actual physical data, purely based on the query used to create the view. There are two types of views:
- Temporary view
- View
==Temporary view -== These views only persist across the lifetime of the pipeline and are not registered in the unity catalog.
==Some deprications:==
![[image 1 2.png]]
### Pipeline Settings
==Pipeline Settings==
==Compute -== Compute resource to run the pipeline
- Serverless - Databricks recommends using serverless. It makes it easier to focus on code rather than the infrastructure.
- Classic Compute (Fixed size) - Autscaling is auto enabled for all new pipelines.
==Code Assets -== Files and modules that make up the pipeline
==Default location for data assets -== Where is data stored by default
==Configuration -== Env variables and parameters
==Tags -== Tags for the pipeline
==Budget -== Defining cost controls and limits
==Advanced Settings==