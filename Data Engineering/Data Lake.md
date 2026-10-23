A data lake is a low-cost, open, durable storage system for any data type - tabular data, text, images, audio, video, JSON, and CSV.
A **data lake** is a centralized storage repository that holds large amounts of raw data in its native format until needed.
In the cloud, every major cloud provider leverages and promotes a data lake, e.g. AWS S3, Azure Data Lake Storage (ADLS), Google Cloud Storage (GCS). As a result, the vast majority of the data of most organizations is stored in cloud data lakes.
**Key characteristics:**
- Stores **structured, semi-structured, and unstructured** data
- Data is kept **as-is** (no upfront schema required)
- Designed for **big data** and analytics workloads
- Supports **batch and real-time** data ingestion
- Typically built on **low-cost object storage** (e.g., S3, Azure Blob)
**Data Lake vs Data Warehouse:**
|Feature|Data Lake|Data Warehouse|
|---|---|---|
|Data format|Raw / unprocessed|Processed / structured|
|Schema|Schema-on-read|Schema-on-write|
|Cost|Lower|Higher|
|Users|Data scientists|Business analysts|
|Flexibility|High|Low|
|Query speed|Slower|Faster|
**Popular platforms:**
|Platform|Provider|
|---|---|
|AWS S3 + Lake Formation|Amazon|
|Azure Data Lake Storage|Microsoft|
|Google Cloud Storage|Google|
|Databricks Lakehouse|Databricks|
|Apache Hadoop HDFS|Open Source|
**India-specific context:**
- Major Indian enterprises (TCS, Infosys, Wipro) offer **data lake implementation services**
- **DigiYatra, ONDC, and GSTN** use data lake architecture to handle massive citizen/transaction data
- Indian cloud regions of AWS, Azure, and GCP all support data lake services locally for **data residency compliance**
#### ==References==
frequentlyask