### read_files
Reads files under a provided location and returns the data in tabular form.
Supports reading `JSON`, `CSV`, `XML`, `TEXT`, `BINARYFILE`, `PARQUET`, `AVRO`, and `ORC` file formats.
==Syntax==
`read_files(path [, option_key => option_value ] [...])`
==Arguments==
`path`: A string with uri location of the files. Supports glob patterns.
`option_key, option_value`: A key value pair of different options available. Refer to the source documentation.
==Usage in streaming tables:==
`read_files` can be used in streaming tables to ingest files into Delta Lake. `read_files` leverages Auto Loader when used in a streaming table query. You must use the `STREAM` keyword with `read_files`. See [What is Auto Loader?](https://docs.databricks.com/aws/en/ingestion/cloud-object-storage/auto-loader/) for more details.

> [!info] read_files table-valued function | Databricks on AWS  
> Learn the syntax of the read\_files function of the SQL language in Databricks SQL and Databricks Runtime.  
> https://docs.databricks.com/aws/en/sql/language-manual/functions/read_files