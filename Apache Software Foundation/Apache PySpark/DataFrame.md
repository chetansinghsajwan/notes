### ==Questions==
- [ ] How to create a data frame
- [ ] Sort the API table
### ==API==
|Syntax|Type|Category|Description|Example|Notes|
|---|---|---|---|---|---|
|`df.columns`|field|Metadata|Returns a list of column names as strings.|`print(df.columns) # ['id','name','age']`|Field, not method|
|`df.write`|field|I/O|Returns a `DataFrameWriter` to save static DataFrames to disk / DB / cloud.|`df.write.mode('overwrite').parquet('/out')`|Static DFs only; field not method|
|`df.writeStream`|field|Streaming|Returns a `DataStreamWriter` for writing streaming DataFrames.|`df.writeStream.format('console').outputMode('append').start()`|Streaming DFs only; field not method|
|`df.sparkSession`|field|Metadata|Returns the `SparkSession` that created this DataFrame.|`spark = df.sparkSession`|Field, not method|
|`df.schema`|field|Metadata|Returns schema as a `StructType` object for programmatic inspection.|`print(df.schema)`|Field, not method|
|`df.isStreaming`|field|Streaming|Returns `True` if DataFrame contains one or more streaming sources.|`if df.isStreaming: print('streaming!')`|Field, not method|
|`df.agg(*exprs)`|method|Aggregation|Aggregate entire DataFrame using one or more aggregate functions.|`df.agg({'age':'max','salary':'mean'})`|Alias for `groupBy().agg()`|
|`df.alias(name)`|method|Transform|Returns a new DataFrame with an alias. Useful in self-joins to disambiguate columns.|`df.alias('emp').join(df.alias('mgr'), col('emp.mgr_id')==col('mgr.id'))`|Returns new DF|
|`df.coalesce(numPartitions)`|method|Transform|Reduces number of partitions without a full shuffle. Only use to reduce, not increase.|`df.coalesce(1).write.csv('/out')`|No shuffle; use `repartition()` to increase|
|`df.collect()`|method|Action|Returns all rows as a list of `Row` objects to the driver.|`rows = df.collect()`|⚠ Pulls all data to driver|
|`df.count()`|method|Action|Returns number of rows as a Python integer. Triggers a full scan.|`n = df.count()`|Full scan — expensive|
|`df.createGlobalTempView(name)`|method|Metadata|Creates a global temp view accessible across SparkSessions until app exits. Throws if exists.|`df.createGlobalTempView('gv')`|Query via `global_temp.<name>`|
|`df.createOrReplaceGlobalTempView(name)`|method|Metadata|Creates or replaces a global temporary view across all SparkSessions.|`df.createOrReplaceGlobalTempView('gv')`|Safe idempotent version|
|`df.createOrReplaceTempView(name)`|method|Metadata|Creates or replaces a session-scoped temporary SQL view. Most commonly used.|`df.createOrReplaceTempView('employees')`|Session-scoped|
|`df.createTempView(name)`|method|Metadata|Creates a session-scoped temporary SQL view. Throws `AnalysisException` if already exists.|`df.createTempView('emp_view')`|Throws if view exists|
|`df.describe(*cols)`|method|Metadata|Computes summary stats (count, mean, stddev, min, max) for numeric and string columns.|`df.describe('age','salary').show()`|Use `summary()` for more stats|
|`df.distinct()`|method|Transform|Returns new DataFrame with duplicate rows removed across all columns.|`df.distinct().count()`|Equivalent to `df.dropDuplicates()`|
|`df.drop(*cols)`|method|Transform|Returns new DataFrame with specified columns removed.|`df.drop('temp_col','audit_ts')`|No error if col absent (Spark 3+)|
|`df.dropDuplicates(subset=None)`|method|Transform|Returns DataFrame with duplicate rows removed. Optionally specify subset of columns.|`df.dropDuplicates(['email'])`|Subset param added Spark 1.4|
|`df.dropDuplicatesWithinWatermark(subset=None)`|method|Streaming|Deduplicates rows within watermark window in streaming DataFrames.|`df.withWatermark('ts','10 minutes').dropDuplicatesWithinWatermark(['id'])`|Spark 3.5+; streaming only|
|`df.dropna(how='any', thresh=None, subset=None)`|method|Transform|Drops rows with null values. `how='any'` drops if any null; `how='all'` only if all nulls.|`df.dropna(subset=['name','email'])`|Alias: `df.na.drop()`|
|`df.exists(f)`|method|Action|Checks if any row satisfies the given predicate function. Experimental.|`df.exists(lambda r: r['age'] > 100)`|Experimental; Spark 3.3+|
|`df.fillna(value, subset=None)`|method|Transform|Replaces null values with specified value — scalar or dict mapping cols to fill values.|`df.fillna({'age':0,'city':'Unknown'})`|Alias: `df.na.fill()`|
|`df.filter(condition)`|method|Transform|Filters rows based on a Column expression or SQL string. Alias for `where()`.|`df.filter(col('age')>18)`|Same as `where()`|
|`df.first()`|method|Action|Returns the first `Row` of the DataFrame. Equivalent to `head(1)[0]`.|`row = df.first()`|Returns `Row`, not DF|
|`df.foreach(f)`|method|Action|Applies function to each Row for side effects (e.g. writing to external systems).|`df.foreach(lambda row: print(row))`|Runs on executors; no return value|
|`df.foreachPartition(f)`|method|Action|Applies function to each partition (iterator of Rows). More efficient than `foreach` for batch writes.|`df.foreachPartition(write_batch)`|Preferred over `foreach` for DB writes|
|`df.groupBy(*cols)`|method|Aggregation|Groups DataFrame by specified columns for aggregation. Returns `GroupedData`.|`df.groupBy('dept').agg(F.sum('salary'))`|Alias: `groupby()`|
|`df.head(n=1)`|method|Action|Returns first n rows as a list of `Row` objects (or single `Row` if n=1).|`df.head(5)`|Returns list if n>1, else `Row`|
|`df.intersect(other)`|method|Transform|Returns rows common to both DataFrames. Removes duplicates. Schemas must match.|`df1.intersect(df2).show()`|Deduplicates result|
|`df.intersectAll(other)`|method|Transform|Returns all rows common to both DataFrames, preserving duplicates.|`df1.intersectAll(df2).show()`|Spark 2.4+; keeps dupes|
|`df.isEmpty()`|method|Action|Returns `True` if the DataFrame has no rows. More efficient than `count()==0`.|`if df.isEmpty(): print('No data')`|Spark 3.3+; prefer over `count()==0`|
|`df.join(other, on=None, how='inner')`|method|Transform|Joins two DataFrames. `how`: inner / outer / left / right / cross / semi / anti.|`df.join(dept_df, df.dept_id==dept_df.id, 'left')`|Use `alias` to avoid ambiguous cols|
|`df.lateralJoin(other)`|method|Transform|Performs a lateral join, allowing reference to left DF columns inside right.|`df.lateralJoin(spark.range(3))`|Spark 3.5+; experimental|
|`df.limit(n)`|method|Transform|Returns new DataFrame with at most n rows. Non-deterministic without sort.|`df.orderBy('ts').limit(100)`|Sort first for determinism|
|`df.mergeInto(target, condition)`|method|I/O|Performs a MERGE INTO / upsert operation. Requires Delta Lake or compatible format.|`df.mergeInto('tgt','src.id=tgt.id').whenMatchedUpdateAll().execute()`|Delta Lake / Iceberg; Spark 4.0+|
|`df.offset(n)`|method|Transform|Returns new DataFrame skipping first n rows. Non-deterministic without sort.|`df.orderBy('id').offset(100).limit(50)`|Spark 3.4+; use with `limit` for pagination|
|`df.orderBy(*cols, ascending=True)`|method|Transform|Returns new DataFrame sorted by specified columns. `ascending` can be bool or list of bools.|`df.orderBy('age', ascending=False)`|Alias: `sort()`|
|`df.pandas_api()`|method|Pandas Bridge|Returns a Pandas-on-Spark DataFrame enabling pandas-style API on Spark data.|`psdf = df.pandas_api()`|Formerly Koalas; Spark 3.2+|
|`df.printSchema()`|method|Metadata|Prints the schema as a tree to stdout.|`df.printSchema()`|Returns `None`; use `df.schema` programmatically|
|`df.replace(to_replace, value, subset=None)`|method|Transform|Replaces a value with another in specified columns (numeric / string / boolean).|`df.replace({'M':'Male','F':'Female'}, subset=['gender'])`|Alias: `df.na.replace()`|
|`df.select(*cols)`|method|Transform|Projects a set of expressions or column names. Most common transformation.|`df.select('name', col('sal')*1.1)`|Returns new DF with only selected cols|
|`df.show(n=20, truncate=True, vertical=False)`|method|Action|Prints first n rows to stdout in tabular form. For display/debugging only.|`df.show(5, truncate=False)`|Not for production pipelines|
|`df.sort(*cols, ascending=True)`|method|Transform|Returns new DataFrame sorted by specified columns. Alias for `orderBy()`.|`df.sort(col('ts').desc())`|Same as `orderBy()`|
|`df.subtract(other)`|method|Transform|Returns rows in first DF not in second. Removes duplicates.|`df_all.subtract(df_processed)`|`exceptAll()` preserves dupes|
|`df.tail(n)`|method|Action|Returns last n rows as a list of `Row` objects. Triggers a full sort — expensive.|`df.tail(3)`|Expensive on large data; Spark 3.0+|
|`df.take(n)`|method|Action|Returns first n rows as a list of `Row` objects. Equivalent to `head(n)`.|`rows = df.take(5)`|Same as `head(n)`|
|`df.to(table)`|method|I/O|Saves DataFrame to a table using column name and type matching.|`df.to('catalog.schema.my_table')`|Spark 3.4+|
|`df.toDF(*cols)`|method|Transform|Returns new DataFrame with columns renamed in positional order.|`df.toDF('emp_id','emp_name','emp_age')`|Column count must match|
|`df.toJSON(use_unicode=True)`|method|Transform|Converts each row to a JSON string. Returns an `RDD[str]`.|`df.toJSON().take(2)`|Returns `RDD[str]`, not DataFrame|
|`df.toLocalIterator(prefetchPartitions=False)`|method|Action|Returns iterator yielding Rows partition by partition. Memory-efficient vs `collect()`.|`for row in df.toLocalIterator(): process(row)`|Prefer over `collect()` for large DFs|
|`df.toPandas()`|method|Pandas Bridge|Converts Spark DataFrame to Pandas DataFrame. Collects all data to driver.|`pdf = df.toPandas()`|⚠ Pulls all data to driver; use on small DFs|
|`df.transform(func, *args, **kwargs)`|method|Transform|Applies a function to the DataFrame and returns result. Enables fluent chaining.|`df.transform(add_flag).transform(add_ts)`|Enables functional/fluent chaining|
|`df.transpose(indexColumn=None)`|method|Transform|Transposes rows to columns. Experimental.|`df.transpose('metric').show()`|Spark 3.6+; experimental|
|`df.union(other)`|method|Transform|Returns new DataFrame with rows from both DFs combined by position. Includes duplicates.|`df1.union(df2)`|By position; use `unionByName` for safety|
|`df.unionAll(other)`|method|Transform|Deprecated alias for `union()`. Kept for backward compatibility.|`df1.unionAll(df2)`|⚠ Deprecated; use `union()` instead|
|`df.unionByName(other, allowMissingColumns=False)`|method|Transform|Unions DataFrames by column name instead of position. Missing cols filled with null.|`df1.unionByName(df2, allowMissingColumns=True)`|Safer than `union()`; Spark 2.3+|
|`df.where(condition)`|method|Transform|Alias of `filter()`.|`df.where(col('active')==True)`|Identical to `filter()`|
|`df.withColumnRenamed(existing, new)`|method|Transform|Returns new DataFrame with one column renamed. No error if column doesn't exist.|`df.withColumnRenamed('dob','date_of_birth')`|Use `withColumnsRenamed` for bulk rename|
|`df.withColumns(colsMap)`|method|Transform|Adds or replaces multiple columns in one call. More efficient than chaining `withColumn()`.|`df.withColumns({'full_name':col('f')+col('l')})`|Spark 3.3+; prefer over repeated `withColumn`|
|`df.withColumnsRenamed(colsMap)`|method|Transform|Renames multiple columns at once using a dict.|`df.withColumnsRenamed({'f':'first','l':'last'})`|Spark 3.4+; bulk rename|
|`df.withWatermark(eventTime, delayThreshold)`|method|Streaming|Defines a watermark on streaming DF to handle late data. Required for stateful ops.|`df.withWatermark('event_ts','15 minutes')`|Streaming only|
|`df.writeTo(table)`|method|I/O|Returns a `DataFrameWriterV2` for catalog table writes with fine-grained control.|`df.writeTo('cat.schema.tbl').createOrReplace()`|V2 API; more powerful than `write.saveAsTable`|