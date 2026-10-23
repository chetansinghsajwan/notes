Lakeguard is a set of technologies on Databricks that enforce code isolation and data filtering so multiple users can share the same compute resource securely and cost-effectively, and access data with fine-grained access controls in place on compute offering privileged machine access.
---
**Simple analogy:**  
Imagine 10 people sharing one office computer. Without Lakeguard — everyone can see each other's files and actions. With Lakeguard — each person is in their own sandbox, sees only what they're permitted to see, even on the same machine.
---
**Two core problems it solves:**
|Problem|Lakeguard solution|
|---|---|
|Multiple users sharing same cluster — can they see each other's data?|Code isolation — users are sandboxed from each other|
|How to enforce row-level and column-level permissions at query time?|Data filtering — policies enforced before data reaches the user|
---
**How it works technically — 2 components:**
Lakeguard builds on two main components: first, it uses Spark Connect (a JDBC-like protocol) to separate the client application from the server and ensure version compatibility. Second, it leverages container isolation in Databricks' cluster manager to securely isolate user code from the core Spark engine.
---
**What Lakeguard enforces:**
|Policy type|Example|
|---|---|
|Row-level filters|User A in Chennai sees only Tamil Nadu data|
|Column masks|SSN column shown as `****-**-1234` for non-privileged users|
|Table-level access|Data analyst cannot query `salary` table|
|Multi-user isolation|Two users on same cluster cannot see each other's variables or data|
---
**Where Lakeguard applies:**
|Compute type|Lakeguard active?|
|---|---|
|Serverless compute (notebooks, jobs)|Yes|
|Standard classic compute (shared)|Yes|
|SQL Warehouses|Yes|
|Single-user classic cluster|Partial — data filtering yes, isolation not needed|
---
**Lakeguard vs Unity Catalog — what's the difference?**
||Unity Catalog|Lakeguard|
|---|---|---|
|What it is|Governance metadata layer — defines permissions|Enforcement engine — actually applies them|
|Role|Stores rules (who can access what)|Executes rules at query time|
|Analogy|Law book|Police officer enforcing the law|
They work together — Unity Catalog defines the policy, Lakeguard enforces it.
---
**In short:** Lakeguard is the **enforcement layer** inside Databricks compute that makes Unity Catalog's governance policies actually work in practice — including when multiple users share the same cluster.