## Topic Overview
Dimensional modelling is a structured approach to designing databases for analytics and reporting, and it is a core topic on the Professional exam. This design methodology organizes data into fact tables (containing metrics and measurements) and dimension tables (containing descriptive attributes). On Databricks, dimensional models are implemented using Delta Lake, which provides the ACID guarantees, time travel, and schema enforcement needed for complex analytical schemas.
The medallion architecture (bronze, silver, gold layers) naturally aligns with dimensional modelling, where the gold layer contains the final dimensional model. Star schemas and snowflake schemas are the two main dimensional patterns, with star schemas preferred for simplicity and query performance, and snowflake schemas used when dimension tables need further normalization. Slowly Changing Dimensions (SCD) are a critical concept: they handle how dimension data changes over time, with Type 1 (overwrite), Type 2 (history with surrogate keys), and Type 3 (current and previous values) being the most common approaches.
On the exam, expect questions about designing star schemas, implementing SCDs using MERGE statements, when to denormalize fact tables, surrogate key generation, and how dimensional models support fast analytical queries and business intelligence tools.
---
## Key Concepts
Star Schema
A fact table sits at the center surrounded by multiple dimension tables connected via foreign keys. The fact table contains measures (aggregatable metrics like sales, quantities) and foreign key references to dimensions. Dimension tables contain the descriptive attributes (customer name, product category, date). This structure enables fast queries because dimension tables are typically small and can be joined efficiently. Star schemas are intuitive for business users and simplify query writing. The design is denormalized, trading some storage space for query performance.
Snowflake Schema
An extension of the star schema where dimension tables are further normalized into sub dimensions. For example, a Product dimension might be split into Product, Category, and Subcategory dimensions, each with its own table and relationships. This reduces redundancy and storage but adds complexity and additional joins. Useful when dimensions have hierarchies or shared attributes across multiple dimensions. However, the extra joins can slow down query performance compared to a fully denormalized star schema. Modern analytical workloads on Databricks typically favor star schemas because storage is cheap and query performance is critical.
Slowly Changing Dimensions (SCD) Types
Type 1: Overwrite the old value with the new value. Used when historical tracking is not required. Type 2: Keep a full history of changes by adding new rows with surrogate keys and effective date ranges. Most common approach for analytics. Allows analysis of how dimensions changed over time. Type 3: Store current and previous values in the same row using multiple columns. Used when you only need to track the most recent change. On Databricks, Type 2 is implemented using MERGE with INSERT and UPDATE operations to handle insertions of new dimension members and updates to existing ones.
Surrogate Keys and Denormalization
Surrogate keys are system generated primary keys (typically auto incrementing integers) that uniquely identify dimension rows. They are preferred over business keys (like customer ID or product SKU) because business keys can change and be reused. Surrogate keys are essential for Type 2 SCDs because they uniquely identify each version of a dimension member. Denormalization in fact tables involves optionally storing frequently accessed dimension attributes directly in the fact table to avoid joins. This is a trade off: denormalization reduces query complexity but increases storage and maintenance overhead. On Databricks with Delta Lake, denormalization decisions depend on query patterns and the cost of computing resources versus storage.
---
## Code Examples
### Star Schema Design
```sql
-- Dimension tables
CREATE TABLE dim_customer (
  customer_key INT PRIMARY KEY,
  customer_id INT,
  customer_name STRING,
  country STRING,
  segment STRING
);
CREATE TABLE dim_product (
  product_key INT PRIMARY KEY,
  product_id INT,
  product_name STRING,
  category STRING,
  unit_price DECIMAL(10, 2)
);
CREATE TABLE dim_date (
  date_key INT PRIMARY KEY,
  full_date DATE,
  year INT,
  month INT,
  day INT
);
-- Fact table
CREATE TABLE fact_sales (
  sales_id INT,
  customer_key INT,
  product_key INT,
  date_key INT,
  quantity INT,
  amount DECIMAL(12, 2),
  FOREIGN KEY (customer_key) REFERENCES dim_customer,
  FOREIGN KEY (product_key) REFERENCES dim_product,
  FOREIGN KEY (date_key) REFERENCES dim_date
);
```
### SCD Type 2 Implementation with MERGE
```sql
-- SCD Type 2: Insert new customer or update existing
MERGE INTO dim_customer t
USING staging_customer s
ON t.customer_id = s.customer_id AND t.is_active = true
WHEN MATCHED AND t.customer_name <> s.customer_name THEN
  UPDATE SET is_active = false, end_date = current_date
WHEN MATCHED AND t.customer_name = s.customer_name THEN
  UPDATE SET last_updated = current_timestamp
WHEN NOT MATCHED THEN
  INSERT (customer_key, customer_id, customer_name, country, is_active, start_date, end_date)
  VALUES (next_val('customer_seq'), s.customer_id, s.customer_name, s.country, true, current_date, NULL);
-- Insert new version with updated attributes
INSERT INTO dim_customer
SELECT next_val('customer_seq'), customer_id, customer_name, country, true, current_date, NULL
FROM staging_customer
WHERE customer_id NOT IN (SELECT customer_id FROM dim_customer);
```
### SCD Type 1: Simple Overwrite
```sql
-- SCD Type 1: Just overwrite the old value
MERGE INTO dim_product t
USING staging_product s
ON t.product_id = s.product_id
WHEN MATCHED THEN
  UPDATE SET
    product_name = s.product_name,
    category = s.category,
    unit_price = s.unit_price,
    last_updated = current_timestamp
WHEN NOT MATCHED THEN
  INSERT (product_key, product_id, product_name, category, unit_price, last_updated)
  VALUES (next_val('product_seq'), s.product_id, s.product_name, s.category, s.unit_price, current_timestamp);
```
### Analytical Query on Star Schema
```sql
-- Query total sales by customer and product
SELECT
  dc.customer_name,
  dp.product_name,
  dd.year,
  SUM(fs.amount) as total_sales,
  SUM(fs.quantity) as total_quantity
FROM fact_sales fs
JOIN dim_customer dc ON fs.customer_key = dc.customer_key
JOIN dim_product dp ON fs.product_key = dp.product_key
JOIN dim_date dd ON fs.date_key = dd.date_key
WHERE dd.year = 2024
GROUP BY dc.customer_name, dp.product_name, dd.year
ORDER BY total_sales DESC;
```
---
## Common Exam Scenarios
### Building a Star Schema for an E commerce Business
You need to design a dimensional model for tracking online sales. The fact table contains order details (quantity, amount, discount). Dimensions include customer (with geographic and demographic attributes), product (with category hierarchy), and date (with fiscal calendar). The candidate should recognize that a star schema is appropriate here because joins are efficient, dimensions are relatively static, and queries benefit from the denormalized structure. Performance is critical for the BI tool connecting to this model.
### Handling Customer Address Changes with SCD Type 2
A customer moves to a new city. Should you overwrite their old address (SCD Type 1) or keep history (SCD Type 2)? The answer depends on analytical requirements. If you need to analyze sales by the customer's address at the time of purchase (historical accuracy), use Type 2 with surrogate keys and date ranges. If you only care about the current address for reporting, Type 1 is simpler. The exam tests whether candidates understand this trade off and can implement MERGE statements correctly.
### Denormalizing a Fact Table for Performance
Queries on a star schema frequently join the fact table with dimensions to retrieve product_category. Rather than always joining, you decide to denormalize by adding product_category directly to the fact table. This eliminates joins but increases storage and requires updating denormalized columns when the dimension changes. The candidate should understand when denormalization is worthwhile (frequent joins on that dimension) versus when it creates maintenance overhead.
---
## Key Takeaways
Star schemas are the foundation of dimensional modelling on Databricks: one fact table surrounded by dimension tables, optimized for fast analytical queries.
Surrogate keys (auto incrementing integers) uniquely identify dimension rows and enable SCD Type 2 implementations. Always use surrogate keys instead of business keys in fact tables.
SCD Type 2 tracks full history of dimension changes using surrogate keys and date ranges. Implement using MERGE: UPDATE old rows to mark as inactive, INSERT new rows with new surrogate keys.
The medallion architecture maps naturally to dimensional modelling: bronze is raw data, silver is cleansed staging, gold is the final dimensional model for analytics.
Denormalize fact tables strategically when specific column combinations are queried frequently. Balance storage costs against query performance and maintenance overhead.
---
## Gotchas and Tips
Business Keys vs Surrogate Keys: Do not use business keys (customer ID, product SKU) directly in fact tables. They can change or be reused. Always use auto incrementing surrogate keys and store business keys in dimension tables for traceability.
SCD Type 2 Complexity: Implementing Type 2 correctly requires careful logic in MERGE statements. Test thoroughly with examples where attributes change and new records are added simultaneously. Mistakes can corrupt dimensional history.
Snowflake Schema Overhead: While snowflake schemas reduce redundancy, the extra joins degrade query performance. Only normalize dimensions if storage or data quality issues justify the additional query complexity.
---
## Links and Resources
- **[Medallion Architecture](https://docs.databricks.com/aws/en/lakehouse/medallion)** — Star schema and dimensional modelling patterns within the medallion architecture.
- **[Delta Best Practices](https://docs.databricks.com/aws/en/delta/best-practices)** — Designing fact and dimension tables optimized for analytical queries.