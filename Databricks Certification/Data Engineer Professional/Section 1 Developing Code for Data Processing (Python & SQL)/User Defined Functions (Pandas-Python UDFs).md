## Topic Overview
User Defined Functions (UDFs) allow you to extend Spark's built in functionality with custom logic written in Python. When Spark's SQL functions and DataFrame transformations are not enough, UDFs let you apply your own logic row by row or across groups of rows.
Databricks supports two main types of Python UDFs: traditional Python UDFs (which serialize Python objects and execute in a Python worker) and Pandas UDFs (which use Arrow columnar transfer and run in vectorized batches). Pandas UDFs are significantly faster because they avoid row by row serialization overhead and can take advantage of vectorized operations. The tradeoff is that Pandas UDFs require your function to work with pandas Series or DataFrames instead of individual Python values.
For the Professional exam, you need to understand when to use Python UDFs vs Pandas UDFs, scalar vs grouped map functions, how to register UDFs for use in SQL, and how to measure the performance difference. The exam expects you to recognize scenarios where Pandas UDFs would outperform traditional UDFs.
---
## Key Concepts
Python UDFs vs Pandas UDFs
Traditional Python UDFs execute row by row. Each row is serialized to Python, passed to your function, and the result is serialized back. This creates significant overhead. Pandas UDFs use Apache Arrow for efficient data transfer and process data in batches (columnar format). A Pandas UDF receives a pandas Series or batch of Series and returns a Series. Because of this vectorized approach, Pandas UDFs are often 5 to 100 times faster than traditional Python UDFs. The tradeoff is that Pandas UDFs require you to write code that works with pandas objects, not scalar Python values.
Scalar vs Grouped Map UDFs
Scalar UDFs (Pandas or Python) take one or more inputs and produce one output per input row. They are suitable for element wise transformations. Grouped map UDFs are Pandas UDFs that partition data by key, receive each group as a separate pandas DataFrame, apply transformations to the entire DataFrame, and return a modified DataFrame. Grouped map UDFs are useful when you need to apply the same complex logic to multiple groups in your data, like rolling window calculations or group specific aggregations.
Registering UDFs for SQL
UDFs defined in Python can be registered as SQL functions using `spark.udf.register()`. This makes the UDF available in SQL queries just like a built in function. When registering, you specify the Python function, a name for the SQL function, and the return type as a Spark SQL type. Once registered, you can use the function in SQL SELECT statements, WHERE clauses, and any other SQL context. This is useful when you want to share custom functions across multiple notebooks or when your pipeline uses both Python and SQL code.
Arrow Optimization for Performance
Arrow is an in memory columnar format that allows efficient transfer of data between Java and Python. Pandas UDFs use Arrow to serialize data from the JVM to Python workers and back. This is much faster than pickling individual row objects. The Arrow format is standardized and optimized for columnar operations, which aligns well with pandas Series and vectorized operations. If you need to call a Python UDF many times or process large datasets, using a Pandas UDF with Arrow serialization can provide massive speedups compared to traditional Python UDFs.
---
## Code Examples
### Traditional Python UDF
```python
from pyspark.sql.functions import col
from pyspark.sql.types import StringType
# Define the UDF
def upper_and_length(s):
    if s is None:
        return None
    return f"{s.upper()}_{len(s)}"
# Create UDF
upper_length_udf = spark.udf.register(
    "upper_length", 
    upper_and_length, 
    StringType()
)
# Use in PySpark
df = spark.createDataFrame([("hello",), ("world",)], ["word"])
result = df.select(upper_length_udf(col("word")).alias("result"))
result.show()
```
### Pandas UDF (Scalar)
```python
from pyspark.sql.functions import pandas_udf
import pandas as pd
from pyspark.sql.types import StringType
# Define Pandas UDF with @decorator
@pandas_udf(StringType())
def upper_and_length_pandas(s: pd.Series) -> pd.Series:
    return s.str.upper() + "_" + s.str.len().astype(str)
# Use in PySpark
df = spark.createDataFrame([("hello",), ("world",)], ["word"])
result = df.select(upper_and_length_pandas("word").alias("result"))
result.show()
# Use in SQL
spark.sql("""
    SELECT upper_and_length_pandas(word) as result FROM your_table
""")
```
### Grouped Map Pandas UDF
```python
from pyspark.sql.functions import col, pandas_udf
from pyspark.sql.types import StructType, StructField, StringType, IntegerType
import pandas as pd
# GroupedMap UDF that normalizes values per group
@pandas_udf(
    StructType([
        StructField("category", StringType()),
        StructField("value", IntegerType()),
        StructField("normalized", IntegerType())
    ])
)
def normalize_per_group(pdf: pd.DataFrame) -> pd.DataFrame:
    max_val = pdf["value"].max()
    pdf["normalized"] = (pdf["value"] * 100) // max_val
    return pdf
# Use with groupedApply
df = spark.createDataFrame(
    [("A", 10), ("A", 20), ("B", 5), ("B", 15)],
    ["category", "value"]
)
result = df.groupby("category").applyInPandas(
    normalize_per_group,
    schema="category string, value int, normalized int"
)
result.show()
```
---
## Common Exam Scenarios
A pipeline processes millions of records and applies a complex string transformation to a column. The current Python UDF implementation is slow. What is the likely issue and solution? The issue is that traditional Python UDFs serialize each row individually, creating massive overhead. The solution is to rewrite the UDF as a Pandas UDF using the @pandas_udf decorator. By processing data in batches using Arrow and pandas vectorized operations, the Pandas UDF will be 10 to 100 times faster. The exam expects you to recognize the performance characteristics of each UDF type.
A data engineer has a Python UDF that performs calculations on individual scalar values and needs to use it in SQL queries. How should they make the UDF available in SQL? They should register the UDF using `spark.udf.register()`, providing the function name, the Python function, and the return type as a Spark SQL type. Once registered, the UDF becomes available as a SQL function that can be called in SELECT, WHERE, and other SQL clauses. This is more convenient than always wrapping SQL data in Python DataFrames.
A grouped aggregation needs to apply custom logic within each group, such as ranking, normalization, or statistical analysis per group. Should you use a scalar Pandas UDF or a grouped map UDF? The correct choice is a grouped map UDF. Scalar UDFs process individual rows and cannot access other rows in the same group. Grouped map UDFs receive each group as a pandas DataFrame, allowing you to perform complex group level operations. You would use `groupby().applyInPandas()` to apply the grouped map UDF.
---
## Key Takeaways
- Pandas UDFs are 5 to 100 times faster than traditional Python UDFs due to Arrow vectorization.
- Use scalar Pandas UDFs for element wise transformations on individual rows or columns.
- Use grouped map Pandas UDFs when you need to apply logic to an entire group of rows at once.
- Register UDFs with `spark.udf.register()` to make them available as SQL functions.
- Always specify the return type explicitly when registering UDFs to help Spark optimize execution.
---
## Gotchas and Tips
⚠️ Pandas UDFs must return the exact output type specified in the decorator. If your function returns a different type, Spark will raise an error at runtime. Always test the return types carefully.
⚠️ Traditional Python UDFs cannot be optimized by Spark's Catalyst optimizer because Spark does not understand Python code. Pandas UDFs are better because they work with Arrow which Catalyst can partially optimize.
⚠️ Grouped map UDFs using `applyInPandas()` trigger a shuffle if the grouping column is not already partitioned. Be aware of this when working with large datasets.
---
## Links and Resources
- **[User-Defined Functions Overview](https://docs.databricks.com/aws/en/udf/)** — Introduction to UDFs in Databricks including scalar, Python, and Pandas UDFs.
- **[Pandas UDFs (Vectorized UDFs)](https://docs.databricks.com/aws/en/udf/pandas)** — High-performance vectorized UDFs using Apache Arrow for batch operations.
- **[Python UDFs](https://docs.databricks.com/aws/en/udf/python)** — Creating and registering Python scalar UDFs for SQL and DataFrame use.