==Homepage:==

> [!info] Unity Catalog  
> Unity Catalog transforms how you manage and govern data, ensuring compatibility and control across diverse platforms and tools.  
> https://www.unitycatalog.io/  
---
Unity catalog is a catalog for data an AI.
- **Multimodal interface supports any format, engine, and asset**
	- Multi-format support: It is extensible and supports Delta Lake, Apache Iceberg and Apache Hudi via UniForm, Apache Parquet, JSON, CSV, and many others.
	- Multi-engine support: Unity Catalog's open APIs allow many leading compute engines to read data cataloged in Unity.
	- Multimodal: It supports all your data and AI assets, including tables, files, functions, AI models.
- **Open source API and implementation** - OpenAPI spec and OSS implementation (Apache 2.0 license). It is also compatible with Apache Hive's metastore API and Apache Iceberg's REST catalog API. Unity Catalog is currently a sandbox project with LF AI and Data Foundation (part of the Linux Foundation).
- **Unified governance** for data and AI - Govern and secure tabular data, unstructured assets, and AI assets with a single interface.
![[image.png]]
---
Unity Catalog is proud to be hosted by the [LF AI & Data Foundation](https://lfaidata.foundation/projects).
---
Unity Catalog stores all assets in a 3-level namespace:
1. catalog
2. schema
3. assets like tables, volumes, functions, etc.
---
==Unity Catalog CLI Usage:==

> [!info] CLI - Unity Catalog  
> Open, Multi-modal Catalog for Data & AI  
> https://docs.unitycatalog.io/usage/cli/  
==Unity Catalog UI Usage:==

> [!info] UI - Unity Catalog  
> Open, Multi-modal Catalog for Data & AI  
> https://docs.unitycatalog.io/usage/ui/  
---
## Challenges using table storage formats
While popular, each table storage format (e.g., Parquet, ORC, JSON, CSV, Avro, TEXT, etc.) has their own set of distinct advantages. But the challenges when working with these formats include:
- No ACID transactions for these data lakes meaning it's easier to accidentally corrupt your data
- It is not easy to delete rows from these tables
- These table storage foramts do not offer DML transactions
- They lack advanced features from schema evolution and enforcement to deletion vectors to change data feed
- Slow file listing overhead when working with cloud object stores such as AWS S3, Azure ADLSgen2, and Google Cloud Storage
- Potentialy expensive footer reads to gather statistics for file skipping
Open table formats like Apache Iceberg and Delta Lake are specifically designed to overcome these challenges. Storing your data in a lakehouse format is almost always more advantageous than storing it in traditional table storage formats.
==What was the need of Unity Catalog? Why was it created?==
### The Core Problem — "Before Unity Catalog"
Before Unity Catalog, Databricks workspaces used separate Hive metastores, leading to:
|Problem|Impact|
|---|---|
|Isolated metadata silos|Data discovery was painful across teams|
|Inconsistent access control|Permissions managed per workspace, not centrally|
|No end-to-end lineage|Impossible to trace data flow for audits or debugging|
|Redundant data copies|Teams duplicated data just to share it|
|Limited cross-team collaboration|No clean way to share across clouds/departments|
|Compliance risks|No visibility or centralized governance|
### The Specific Technical Pain Points
- If you had a hundred workspaces sharing a single data source, you'd end up managing a hundred individual sets of permissions — like juggling a hundred keys for a single door.
- Mount points — used to access the data lake — were available at the workspace level and couldn't be restricted to specific users or groups, meaning anyone with workspace access could potentially modify data.
- The old Hive metastore lacked key features like data lineage, access patterns, and data discovery — capabilities crucial for modern governance.
### Why It Matters (Business Angle)
- Enterprises spend 20–40% of their IT budgets on fixing data governance issues.
- Without Unity Catalog, organizations using multiple cloud providers (AWS, Azure, GCP) had no single standard — each cloud's security provider worked differently.
- It enables compliance with regulations like GDPR, HIPAA, and CCPA through centralized access control, auditing, and lineage tracking.
### India-Specific Relevance
- Indian enterprises in **BFSI (banking/finance)** and **healthcare** are especially impacted — these sectors face strict RBI, SEBI, and DPDP Act 2023 compliance requirements where data lineage and access audits are mandatory.
- Large Indian IT firms (TCS, Infosys, Wipro) operating multi-cloud environments for clients directly benefit from Unity Catalog's cloud-agnostic governance.
- Without it, a bank with 50+ Databricks workspaces would need manual, error-prone access management — a serious regulatory risk.
### In One Line
Unity Catalog was created because **as data grew across multiple clouds, teams, and platforms, governance became a fragmented mess** — and enterprises needed one single, consistent, auditable control plane for all their data and AI assets.
==I get it that Unity Catalog was created to cover the shortcomings of the hive metastore. but why does databricks need a catalog in the first place. i dont see databases like postgres to use a catalog?==
The key difference is **where your data actually lives**.
|Aspect|Postgres|Databricks|
|---|---|---|
|Data lives in|Postgres itself (it owns the files)|External cloud storage (S3, ADLS, GCS)|
|Access control|Postgres manages everything|Cloud storage + compute are separate|
|Users|Connect to one DB engine|100s of engineers, data scientists, analysts across teams|
|Data formats|Only its own format|Parquet, CSV, Delta, Iceberg, ORC, JSON...|
|Scale|One server/cluster|Dozens of workspaces, multiple clouds|
|AI/ML assets|Not applicable|Models, notebooks, features also need governance|
### The Core Reason
In Postgres, the **database engine owns the data**. It controls who sees what, where files are, what format they're in — everything in one place. No catalog needed because Postgres _is_ the catalog.
In Databricks, **compute and storage are completely separated**:
- Your data sits in S3 / Azure Data Lake / GCS (owned by your cloud account)
- Databricks just _borrows_ that data to run queries
- So something external needs to track: _what data exists, where it is, who can access it_
That "something" is the catalog.
### Analogy
Postgres is like a **private office** — you own the room, the files, the lock.
Databricks is like a **co-working space** — 500 people, files scattered across 10 storage rooms, different teams needing different access. You need a **reception desk** (catalog) that knows where everything is and who's allowed where.
### Bottom Line
Databases like Postgres don't need a catalog because they are self-contained. Databricks operates on top of raw cloud storage with massive teams and diverse data — so a catalog is essential just to answer the basic question: _"what data do we have, where is it, and who can touch it?"_
==Audit logs as of 2026-04-17?==
Good clarification. You're asking about **open-source Unity Catalog** (the standalone project, not Databricks-hosted).Honest answer — **audit logs in OSS Unity Catalog are very immature right now.**
### Current State of Audit Logs in OSS Unity Catalog
Auditing actions do exist in OSS Unity Catalog. It logs each action in a `.txt` file, but the logs are not structured and are hard to interpret.
So to directly answer your question — you find them in a **plain text log file** on the server where Unity Catalog is running. There is no UI, no dashboard, no query interface for it yet.
### Why It's Still Primitive
Governance-related features like RBAC, auditing, and lineage were planned for release around v0.5 of the OSS project. The OSS version is still catching up to the commercial Databricks version in terms of governance maturity.
### What OSS Unity Catalog Currently Has vs Doesn't Have
|Feature|OSS Unity Catalog|
|---|---|
|Audit logs|⚠️ Exists but unstructured `.txt` file|
|RBAC / access control|✅ Basic support|
|Data lineage|❌ Not available|
|Lineage visualization|❌ Not available|
|Automated sensitive data discovery|❌ Not available|
|UI for audit logs|❌ Not available|
|Structured/queryable audit logs|❌ Not available|
### Bottom Line
If audit logs are important to you, OSS Unity Catalog is **not production-ready** for that use case yet. It's still a sandbox/community project under the Linux Foundation. The rich audit logging you might read about online almost always refers to **Databricks Unity Catalog**, not the OSS version.
# Related
[[Metastore|Apache Hive Metastore]]
# References

> [!info] Unity Catalog  
> Open, Multi-modal Catalog for Data & AI  
> https://docs.unitycatalog.io/