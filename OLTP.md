**OLTP (Online Transaction Processing)** — systems designed to manage real-time, day-to-day transactional data.
---
**Core characteristics:**
- High volume of short, fast transactions (INSERT, UPDATE, DELETE)
- Data is current, not historical
- Highly normalized schema (3NF) to avoid redundancy
- Optimized for writes
- Many concurrent users
---
**ACID properties (what makes OLTP reliable):**
|**Property**|**Meaning**|
|---|---|
|**Atomicity**|Transaction fully completes or fully rolls back|
|**Consistency**|Data always moves from one valid state to another|
|**Isolation**|Concurrent transactions don't interfere|
|**Durability**|Committed data survives crashes|
---
**Schema style — Normalized (3NF):**
- Data split into many related tables
- Minimizes duplication
- Example: `orders` → `order_items` → `products` → `customers`
---
**Popular OLTP databases:**
|Database|Type|India usage|Approx. licensing (INR)|
|---|---|---|---|
|MySQL|Open source|Very common (startups)|Free|
|PostgreSQL|Open source|Growing fast|Free|
|Oracle DB|Enterprise|Banks, PSUs, legacy|₹5L–₹50L+/year|
|Microsoft SQL Server|Enterprise|Mid-large enterprises|₹1.5L–₹20L+/year|
|Amazon RDS|Cloud managed|Startups, SaaS|Pay-per-use|
---
**OLTP vs OLAP (quick recap):**
|Aspect|OLTP|OLAP|
|---|---|---|
|Operations|INSERT/UPDATE/DELETE|SELECT (complex)|
|Users|Thousands (end users)|Dozens (analysts)|
|Data volume per query|Few rows|Millions of rows|
|Response time|Milliseconds|Seconds to minutes|
|Schema|Normalized|Denormalized|
|Example|UPI payment, booking|Sales trend report|
---
**Real-world OLTP examples in India:**
- UPI transactions (PhonePe, GPay) — millions of TPS
- IRCTC ticket booking
- Bank core banking systems (Finacle — heavily used by Indian banks)
- Zerodha/Groww order placement
---
**OLTP → Data Warehouse pipeline:**
```
OLTP DB  →  ETL/ELT  →  Data Warehouse (OLAP)
(real-time ops)         (historical analytics)
```
In your stack, this would look like:  
`Source DB → Ingestion (ADF/Spark) → Delta Lake (Bronze → Silver → Gold)`