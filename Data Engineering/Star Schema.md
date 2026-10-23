A **star schema** is a database design pattern used in data warehousing where one central **fact table** connects to multiple **dimension tables** — visually resembling a star.
Its primary goal is to structure large datasets intuitively to optimize for analysis.
It requires modelers to classify their model tables as either _dimension_ or _fact_.
#### ==**Dimension tables**==
- _Dimension tables_ describe business entities—the _things_ you model. Entities can include products, people, places, and concepts including time itself.
- A dimension table contains a key column (or columns) that acts as a unique identifier, and other columns. Other columns support filtering and grouping your data. The most consistent table you'll find in a star schema is a date dimension table.
#### ==**Fact tables**==
- _Fact tables_ store observations or events, and can be sales orders, stock balances, exchange rates, temperatures, and more.
- A fact table contains dimension key columns that relate to dimension tables, and numeric measure columns.
- The dimension key columns determine the _dimensionality_ of a fact table, while the dimension key values determine the _granularity_ of a fact table.
- For example, consider a fact table designed to store sale targets that has two dimension key columns `Date` and `ProductKey`. It's easy to understand that the table has two dimensions. The granularity, however, can't be determined without considering the dimension key values. In this example, consider that the values stored in the `Date` column are the first day of each month. In this case, the granularity is at month-product level.
![[image 1.png]]
### ==Who created star schema?==
- Introduced by Ralph Kimball in 1996 in his book ==The Data Warehouse Toolkit (Wiley, 2013 — 3rd edition)==. Ralph Kimball is widely regarded as the ==Father of the star schema==.
- Also known ==Kimball Data Model==.
- Ralph didn't invent the original basic concepts of facts and dimensions, but he established an extensive portfolio of dimensional techniques and vocabulary, including conformed dimensions, slowly changing dimensions, junk dimensions, and more.
- Over the past 30 years, it has evolved to become the standard design for dimensional modeling used across a multitude of industries.
### ==Benifits==
1. ==**Fewer Joins = Faster Queries**==
	- In a **normalized (3NF) model**, to get "sales by city", you'd join: Sales → Customer → Address → City — **4 tables**
	- In star schema, City is already **inside the Customer dimension** — just **1 join** from fact table
	- Fewer joins = less CPU + memory = faster results
2. ==**Pre-Aggregated / Denormalized Data**==
	- Dimension tables store **redundant but ready-to-use** data
	- Example: `dim_product` already has `category`, `brand`, `subcategory` in one row
	- No need to compute these at query time — database just **scans and filters**
3. ==**Columnar Storage Advantage**==
	- Fact tables have **millions of rows but queries only need a few columns** (e.g., `sales_amount`, `date_id`)
	- Columnar databases (BigQuery, Redshift) **read only the needed columns**, skipping the rest
	- Star schema's wide fact table is **perfectly shaped** for this — direct column reads, no reconstruction needed
4. ==**Partition & Indexing Efficiency**==
	- Fact tables are typically **partitioned by date** (a dimension key)
	- Query like "sales in Jan 2024" skips all other partitions entirely — called **partition pruning**
	- Dimension foreign keys in fact table act as **natural indexes**
5. ==**BI Tool Optimization**==
	- Tools like Power BI / Tableau **auto-detect** fact vs dimension tables
	- They build **in-memory aggregations (aggregation cache)** along dimension lines
	- Result: second query on same dimension is served from **cache, not database**
6. ==**Simple Mental Model for Aggregation**==
    
	- All metrics (revenue, quantity) sit in **one fact table**
	- All filters (region, time, product) sit in **dimension tables**
	- SQL becomes predictable:
    
	```sql
	-- Always this pattern:
	SELECT d.region, SUM(f.sales)
	FROM fact_sales f
	JOIN dim_region d ON f.region_id = d.region_id
	GROUP BY d.region
	```
    
	No complex subqueries or CTEs needed for basic reporting
    
7. ==**Historical Data via SCD (Slowly Changing Dimensions)**==
	- Dimensions change over time — e.g., customer moves city
	- Star schema handles this via **SCD Type 2**: adds a **new row** with effective dates instead of overwriting
	- So old sales still correctly show **old city**, new sales show **new city** — history is preserved at the row level, not just logged
# ==References==

> [!info] What is Star Schema? | Databricks  
> What is a star sche  
> https://www.databricks.com/blog/what-is-star-schema  
# ==Must Read==

> [!info] What is Star Schema? | Databricks  
> What is a star sche  
> https://www.databricks.com/blog/what-is-star-schema  
- Explains star schema

> [!info] Understand star schema and the importance for Power BI - Power BI  
> Understand star schema design and its relevance to developing Power BI semantic models optimized for performance and usability.  
> https://learn.microsoft.com/en-us/power-bi/guidance/star-schema  
- Explains star schema in detail and concepts related to star schema