It is just an extension of star schema where dimension tables are **further normalized** into sub-dimension tables — visually resembling a snowflake.
![[image 2.png]]
### ==How it Actually Works==
**Star Schema** — `dim_product` is flat:
```
dim_product
-----------
product_id
product_name
category_name      ← redundant in every row
subcategory_name   ← redundant in every row
brand_name         ← redundant in every row
```
**Snowflake Schema** — `dim_product` is split:
```
dim_product          dim_category         dim_brand
-----------          ------------         ---------
product_id    →      category_id    →     brand_id
product_name         category_name        brand_name
category_id          subcategory_id
brand_id
```
- Now to get "sales by category", you join: `fact_sales → dim_product → dim_category` — **extra hop**
### ==Structure Comparison==
|Aspect|Star Schema|Snowflake Schema|
|---|---|---|
|Dimension tables|Denormalized (flat)|Normalized (split further)|
|Joins needed|Less|More|
|Query speed|Faster|Slower|
|Storage|More|Less|
|Complexity|Simple|Complex|
|Data redundancy|High|Low|
### ==Why Normalize Dimensions?==
- `category_name` like "Electronics" repeated **10 million times** in star schema
- In snowflake, it's stored **once** in `dim_category`
- Saves significant storage at scale
### ==Actual Query Difference==
**Star Schema query:**
```sql
SELECT p.category_name, SUM(f.sales_amount)
FROM fact_sales f
JOIN dim_product p ON f.product_id = p.product_id
GROUP BY p.category_name
```
**Snowflake Schema query:**
```sql
SELECT c.category_name, SUM(f.sales_amount)
FROM fact_sales f
JOIN dim_product p ON f.product_id = p.product_id
JOIN dim_category c ON p.category_id = c.category_id  ← extra join
GROUP BY c.category_name
```
### ==When to Use Snowflake Over Star==
|Situation|Use Snowflake|
|---|---|
|Storage is expensive|✅ Yes|
|Dimensions are huge (millions of rows)|✅ Yes|
|Data changes frequently|✅ Yes|
|Query speed is top priority|❌ No, use Star|
|BI users are non-technical|❌ No, use Star|
|ETL pipelines are complex|❌ No, use Star|
### ==Advantages of Snowflake over Star Schema==
|Advantage|How Actually|
|---|---|
|**Less storage**|"Electronics" stored once in `dim_category`, not repeated 10M times in `dim_product`|
|**Better data integrity**|Update category name in 1 row in `dim_category` — reflects everywhere automatically|
|**Handles large dimensions**|When `dim_product` has 50M rows, splitting it reduces each table's size dramatically|
|**Easier dimension updates**|Change 1 row in `dim_brand` vs updating millions of rows in a flat dimension|
|**More accurate modeling**|Real-world hierarchies (State→District→Taluka) map naturally to snowflake structure|
### ==Disadvantages of Snowflake over Star Schema==
|Disadvantage|How Actually|
|---|---|
|**Slower queries**|Every extra join = extra hash/merge operation in query engine — compounds at billions of rows|
|**Complex ETL**|Loading data means maintaining insert order — `dim_category` must load before `dim_product` (foreign key dependency)|
|**Harder for BI tools**|Power BI / Tableau need manual relationship mapping — star schema is auto-detected|
|**More complex SQL**|Simple "sales by brand" now needs 3 joins instead of 1 — mistakes are easier|
|**Query optimization harder**|Query planner has more join paths to evaluate — execution plans become unpredictable|
|**Cache inefficiency**|BI in-memory cache works best on flat wide tables — snowflake's split tables reduce cache hit rate|
### ==Who created Snowflake Schema?==
Snowflake schema is not created by a single person, instead it has evolved from the Star Schema in the data industry.

> Important distinction here — **snowflake schema** and **Snowflake (the company)** are two completely different things.
# ==Must Read==

> [!info] What is Snowflake Schema? | Databricks  
> A snowflake schema is a multi-dimensional data model that is an extension of a star schema, where dimension tables are broken down into subdimensions.  
> https://www.databricks.com/blog/what-is-snowflake-schema