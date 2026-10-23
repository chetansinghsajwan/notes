## Topic Overview
PySpark DataFrames provide a powerful set of aggregation methods that let you compute summaries across groups of data. Whether you're calculating totals, averages, or ranking records within groups, the aggregation API gives you the tools to express complex analytical queries efficiently. These operations are central to data processing tasks in Databricks, from simple counts to advanced window functions.
The core of PySpark aggregation revolves around groupBy() to partition data and agg() to apply aggregate functions. Understanding when to use built in functions versus window functions versus pivot operations is critical for writing efficient, readable code that passes the exam.
This topic covers the core aggregation API, window functions for ranking and running calculations, pivot tables for reshaping data, and multi dimensional aggregations with cube() and rollup(). You'll see exam questions asking you to choose the right aggregation method for a given scenario.
---
## Key Concepts
groupBy() and agg() Basics
The groupBy() method returns a GroupedData object that represents rows grouped by one or more columns. You must chain an aggregation function (like agg(), count(), or sum()) to trigger the computation. The agg() method accepts multiple functions at once using column expressions from pyspark.sql.functions. This lets you compute several aggregates in a single pass over the data, which is more efficient than computing them separately.
Window Functions
Window functions compute values over a specified range of rows (a window) without reducing the result set size. Define a window using Window.partitionBy().orderBy() and apply functions like F.row_number(), F.rank(), F.lag(), F.lead(), or even F.sum().over(window). Useful for ranking items within groups, finding top N records, or computing running totals.
pivot() for Data Reshaping
The pivot() method rotates rows into columns. Typically used with groupBy() to transform long format data into wide format. Example: df.groupBy("category").pivot("month").sum("revenue") creates columns for each month with summed revenue values.
cube() and rollup() for OLAP Analysis
Both cube() and rollup() compute aggregates at multiple levels of grouping. rollup() creates subtotals in a hierarchical manner (parent to child). cube() computes aggregates for all possible combinations of columns (more granular but also more expensive). Use rollup() when you need hierarchical totals. Use cube() when you need to analyze across all dimension combinations.
Column Aliasing and Renaming
Use .alias("new_name") to rename aggregation results. This is especially important when using multiple aggregation functions on the same column, as default names become verbose (e.g., sum(revenue) instead of auto generated names). Good naming makes your results readable and your code easier to maintain.
Sorting Aggregated Results
After aggregation, use orderBy() or sort() to order the results. These methods work on the aggregated DataFrame just like on any other DataFrame. Common exam scenarios ask you to find top N values after grouping and aggregating, which usually requires sorting first.
---
## Code Examples
### Basic groupBy with Multiple Aggregations
```python
from pyspark.sql import functions as F
# Group by product category and calculate multiple aggregates
df.groupBy("category").agg(
    F.count("*").alias("total_orders"),
    F.sum("amount").alias("total_revenue"),
    F.avg("amount").alias("avg_order_value"),
    F.min("amount").alias("min_order"),
    F.max("amount").alias("max_order")
).show()
```
### Window Function for Running Total and Rank
```python
from pyspark.sql.window import Window
from pyspark.sql import functions as F
# Define window partitioned by customer, ordered by date
window_spec = Window.partitionBy("customer_id").orderBy("purchase_date")
df_with_window = df.withColumn(
    "running_total", F.sum("amount").over(window_spec)
).withColumn(
    "purchase_rank", F.rank().over(window_spec)
).withColumn(
    "previous_amount", F.lag("amount", 1).over(window_spec)
).withColumn(
    "next_amount", F.lead("amount", 1).over(window_spec)
)
df_with_window.show()
```
### Pivot Table Example
```python
# Transform long format data to wide format
# Input: rows with (product, month, revenue)
# Output: columns for each month with revenue as values
pivoted_df = df.groupBy("product").pivot("month").sum("revenue")
pivoted_df.show()
# You can specify the values for the pivot column to limit columns
pivoted_df = df.groupBy("product").pivot("month", ["Jan", "Feb", "Mar"]).sum("revenue")
pivoted_df.show()
```
### Finding Top N per Group using row_number()
```python
from pyspark.sql.window import Window
from pyspark.sql import functions as F
# Get the top 3 products by revenue within each region
window_spec = Window.partitionBy("region").orderBy(F.desc("revenue"))
top_products = df.withColumn(
    "rank", F.row_number().over(window_spec)
).filter(F.col("rank") <= 3)
top_products.show()
# Note: use row_number() for strict ranking (1,2,3)
# Use rank() for ties (1,1,3) or dense_rank() for ties (1,1,2)
```
### Equivalent SQL Comparison
```sql
-- PySpark groupBy().agg() equivalent
SELECT 
  category,
  COUNT(*) as total_orders,
  SUM(amount) as total_revenue,
  AVG(amount) as avg_order_value
FROM orders
GROUP BY category;
-- Window function equivalent
SELECT 
  customer_id,
  purchase_date,
  amount,
  SUM(amount) OVER (PARTITION BY customer_id ORDER BY purchase_date) as running_total,
  RANK() OVER (PARTITION BY customer_id ORDER BY purchase_date) as purchase_rank
FROM orders;
-- Pivot equivalent
SELECT *
FROM (
  SELECT product, month, revenue FROM sales
)
PIVOT (
  SUM(revenue) FOR month IN ('Jan', 'Feb', 'Mar')
);
```
---
## Common Exam Scenarios
### Scenario 1: Counting Distinct Values with Multiple Aggregates
Question: You have an orders table with columns (customer_id, product_id, order_amount, order_date). Your task is to group by product_id and calculate the total revenue, number of distinct customers, and count of orders.
Solution: Use groupBy("product_id") with agg() and apply F.sum("order_amount"), F.countDistinct("customer_id"), and F.count("*"). The key is using countDistinct() to count unique customers, not total orders. Many candidates forget that a regular count() would count all rows, not distinct customers.
### Scenario 2: Finding Top N Records per Group
Question: You need to find the top 2 highest selling products (by total sales amount) within each region. The data has (region, product, sales_amount).
Solution: First aggregate by region and product using groupBy().agg() to sum sales. Then use a window function with Window.partitionBy("region").orderBy(F.desc("total_sales")) and F.row_number() to rank within each region. Filter where rank <= 2. This is a two step process: aggregate first, then rank. Confusing the order (trying to rank before aggregating) is a common mistake.
### Scenario 3: Pivoting Data for Cross Tabulation
Question: You have sales data with (salesperson, quarter, amount). You need to reshape it so each quarter becomes a column showing that salesperson's sales for that quarter.
Solution: Use df.groupBy("salesperson").pivot("quarter").sum("amount"). The pivot() method automatically creates columns for each unique value in the quarter column. If you want to control which columns appear (and their order), pass a list: .pivot("quarter", ["Q1", "Q2", "Q3", "Q4"]). Note that pivot operations can be memory intensive with high cardinality columns, which is why specifying values upfront is often recommended.
---
## Key Takeaways
- groupBy() always requires a downstream aggregation function like agg(), count(), or sum(). Forgetting this is a common mistake that will cause your code to not execute.
- Use agg() with multiple functions (F.sum(), F.avg(), F.count(), etc.) to compute several aggregates in a single pass, which is more efficient than chaining multiple groupBy operations.
- Window functions compute over a specified range without reducing row count. They're essential for ranking, running totals, and comparing rows within groups.
- Choose the right ranking function: row_number() gives 1,2,3; rank() gives 1,1,3; dense_rank() gives 1,1,2. Know the difference for exam questions.
- pivot() reshapes data from long to wide format. Always specify the pivot values as a list to control columns and avoid memory issues.
---
## Gotchas and Tips
Gotcha 1: Forgetting to chain an aggregation function after groupBy(). If you write df.groupBy("category") without calling agg() or count(), the code won't fail but it also won't compute anything. Always follow groupBy() with an aggregation method.
Gotcha 2: Confusing count() with countDistinct(). The count() function counts rows. The countDistinct() function counts unique values. Many candidates use count() when they meant to count distinct customers or products, leading to wrong results.
Gotcha 3: Using pivot() on high cardinality columns. If the pivot column has thousands of unique values, the operation becomes very memory intensive or even fails. Always specify the pivot values explicitly as a list to limit the output columns.
---
## Links and Resources
- **[Tutorial: Query and visualize data from a notebook](https://docs.databricks.com/aws/en/getting-started/quick-start)** — Getting started tutorial covering DataFrame operations and visualizations.
- **[Higher-order functions](https://docs.databricks.com/aws/en/semi-structured/higher-order-functions)** — Working with complex data types using TRANSFORM, FILTER, and AGGREGATE.