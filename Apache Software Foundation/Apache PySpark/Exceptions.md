> Source: PySpark 4.1.1 official docs. Sorted by category, then alphabetically.
## Hierarchy
```
Exception (Python built-in)
└── PySparkException
    ├── AnalysisException
    │   ├── ParseException
    │   └── TempTableAlreadyExistsException
    ├── IllegalArgumentException
    ├── IllegalStateException
    ├── StreamingQueryException
    ├── UnknownException
    ├── UnsupportedOperationException
    └── SparkException
        ├── ClassNotFoundException
        ├── PythonException
        ├── QueryExecutionException
        ├── SparkUpgradeException
        └── SparkRuntimeException
            ├── ArithmeticException
            ├── ArrayIndexOutOfBoundsException
            ├── DateTimeException
            ├── IndexOutOfBoundsException
            ├── NumberFormatException
            └── SparkNoSuchElementException
Exception (Python built-in)           ← separate tree
└── Py4JError
    ├── Py4JJavaError
    └── Py4JNetworkError
```
---
## Exception Table
All the following exceptions are present in the module `pyspark.errors`.
|Exception|Direct Parent|Category|When Raised|Common Cause|How to Handle|
|---|---|---|---|---|---|
|`AnalysisException`|`PySparkException`|SQL / Plan Analysis|Spark cannot resolve or analyze the query plan|Invalid column name, wrong schema, missing table, ambiguous reference|Validate column names and schema before operations|
|`ArithmeticException`|`SparkRuntimeException`|Runtime|Arithmetic error during execution|Division by zero in SQL expressions|Use `nullif(col, 0)` to guard divisors|
|`ArrayIndexOutOfBoundsException`|`SparkRuntimeException`|Runtime|Array index access is out of bounds|Accessing `array[n]` where n >= array size|Check array size before indexing|
|`ClassNotFoundException`|`SparkException`|Configuration|A required Java/Scala class is not found on the classpath|Missing JAR, wrong connector class name|Add missing JAR to `spark.jars` config|
|`DateTimeException`|`SparkRuntimeException`|Runtime|Invalid date/time value during parsing or computation|Bad date strings, out-of-range timestamps|Use `try_cast` or `to_date` with `mode='PERMISSIVE'`|
|`IllegalArgumentException`|`PySparkException`|API Misuse|Illegal or inappropriate argument passed to a Spark function|Negative sample fraction, invalid config value|Validate input arguments before calling Spark APIs|
|`IllegalStateException`|`PySparkException`|State|Operation called on an object in an invalid state|Accessing a closed SparkContext or stream|Check object lifecycle before calling methods|
|`IndexOutOfBoundsException`|`SparkRuntimeException`|Runtime|General index out-of-bounds access|Row/partition index access errors|Validate sizes before indexing|
|`NumberFormatException`|`SparkRuntimeException`|Runtime|String cannot be parsed as a number|Casting non-numeric strings to int/float|Use `try_cast` instead of `cast`|
|`ParseException`|`AnalysisException`|SQL Parsing|SQL string cannot be parsed|Syntax error in `spark.sql()` string|Validate SQL syntax; use `explain()` to debug|
|`PySparkException`|`Exception`|Base|Base class for all PySpark exceptions|—|Catch as a fallback for any PySpark error|
|`PythonException`|`SparkException`|UDF / Worker|Exception thrown from a Python worker (executor side)|Error inside a UDF, `map()`, `foreachPartition()`|Check UDF logic; look at `cause` attribute for root error|
|`QueryExecutionException`|`SparkException`|Execution|Error during physical query execution|OOM, corrupt data file, task failure|Check executor logs; increase memory or check data|
|`SparkException`|`PySparkException`|General|General Spark engine error|Broad execution failures, task errors|Check full stack trace; often wraps a Java exception|
|`SparkNoSuchElementException`|`SparkRuntimeException`|Runtime|Accessing an element that does not exist|`map.get(key)` in Spark SQL on a missing key|Use `map_contains_key()` before access|
|`SparkRuntimeException`|`SparkException`|Runtime|Runtime error from Spark engine|ANSI mode type violations, null access in strict mode|Disable ANSI mode or fix data quality issues|
|`SparkUpgradeException`|`SparkException`|Compatibility|Behaviour changed between Spark versions|Date/time format changes from Spark 2.x → 3.x|Follow migration guide; set legacy configs if needed|
|`StreamingQueryException`|`PySparkException`|Streaming|A streaming query fails or is terminated with an error|Source unavailable, watermark issues, bad data in stream|Use `query.exception()` to inspect; handle in `awaitTermination()`|
|`TempTableAlreadyExistsException`|`AnalysisException`|Metadata|Creating a temp view that already exists|Calling `createTempView()` twice with same name|Use `createOrReplaceTempView()` instead|
|`UnknownException`|`PySparkException`|Unknown|Unexpected or unclassified error|Rare internal errors|Log and escalate; check Spark logs|
|`UnsupportedOperationException`|`PySparkException`|API Misuse|Operation is not supported in current context|Calling a non-streaming API on a streaming DF, or vice versa|Check `df.isStreaming` before calling stream-specific APIs|
## Py4J Exceptions (JVM Bridge)
All the following errors are in the module `py4j.protocol`.
|Exception|Direct Parent|When Raised|Notes|
|---|---|---|---|
|`Py4JError`|`Exception`|Python client tries to access an object that no longer exists on JVM side|Less common; usually after SparkContext is stopped|
|`Py4JJavaError`|`Py4JError`|A Java exception is thrown on the JVM side|Most common wrapper for JVM errors; check `.java_exception` for root cause|
|`Py4JNetworkError`|`Py4JError`|Network error between Python driver and JVM|Check JVM process is running; may indicate OOM crash|
## Key Exception Methods (Spark 3.4+)
|Method|Description|
|---|---|
|`e.getMessage()`|Full error message string|
|`e.getCondition()`|Error condition/class identifier (Spark 4.0+; replaces `getErrorClass`)|
|`e.getErrorClass()`|Error class string — **deprecated in Spark 4.0**, use `getCondition()`|
|`e.getMessageParameters()`|Dict of parameters used in the error message template|
|`e.getSqlState()`|SQLSTATE code (standard SQL error code)|
|`e.getQueryContext()`|Query context (e.g. which SQL fragment caused the error)|
---
## Common Error Handling Patterns
```python
from pyspark.errors import AnalysisException, ParseException, SparkException
from py4j.protocol import Py4JJavaError
# Pattern 1 — catch specific PySpark exceptions
try:
    df.select('bad_col').show()
except AnalysisException as e:
    print(f"Analysis error: {e.getMessage()}")
# Pattern 2 — catch JVM-side exceptions
try:
    spark.sql("SELECT * FROM non_existent_table")
except Py4JJavaError as e:
    print(f"Java error: {e.java_exception}")
# Pattern 3 — catch all PySpark exceptions
from pyspark.errors import PySparkException
try:
    df.count()
except PySparkException as e:
    print(f"PySpark error [{e.getCondition()}]: {e.getMessage()}")
# Pattern 4 — avoid TempTableAlreadyExistsException
df.createOrReplaceTempView('my_view')   # safe; always use this
```
---
## Legacy Import Path (still works, but prefer `pyspark.errors`)
```python
# Old style (Spark < 3.3)
from pyspark.sql.utils import AnalysisException, ParseException, IllegalArgumentException
# New style (Spark 3.3+)
from pyspark.errors import AnalysisException, ParseException, IllegalArgumentException
```
---
Py4J is a bridge library that allows Python to talk to a JVM (Java Virtual Machine). PySpark uses it under the hood to communicate between the Python driver and the Spark engine running on Java/Scala.
**The 3 exceptions:**
- `Py4JError` — base class. Raised when something goes wrong on the Python side of the bridge, e.g. trying to access a Java object that no longer exists (like after SparkContext is stopped).
- `Py4JJavaError` — the one you'll see most often. Raised when the Java/Scala side throws an exception. The actual root cause lives in `.java_exception`, and the real Spark error (e.g. `AnalysisException`) is usually buried inside that. This is why PySpark stack traces are so long — it's a Python wrapper around a Java wrapper around the actual error.
- `Py4JNetworkError` — raised when the network connection between the Python process and the JVM process breaks. Usually means the JVM crashed (often OOM), the executor died, or there's a misconfiguration.
**Why you need to know this:**
In older Spark versions (pre-3.3), many PySpark exceptions like `AnalysisException` were actually surfaced _as_ `Py4JJavaError`, not as native Python exceptions. So you'd have to catch `Py4JJavaError` and parse the string message to figure out what went wrong. From Spark 3.3+ onwards, PySpark wraps these properly into native exceptions like `AnalysisException`, so you rarely need to catch `Py4JJavaError` directly anymore — but it's still good to have as a fallback.
```python
from py4j.protocol import Py4JJavaError
try:
    spark.sql("SELECT * FROM missing_table")
except Py4JJavaError as e:
    print(e.java_exception)   # dig into the Java stack trace
```