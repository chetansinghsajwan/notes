## Topic Overview
Data quality expectations are critical in production pipelines. You cannot let bad data silently poison your tables. You need to define what "good" data looks like, catch violations, and decide whether to quarantine, drop, or fail.
Lakeflow provides the expect and expect_or_drop APIs to define quality checks as part of the pipeline. Auto Loader rescues unparseable records to the _rescued_data column instead of failing. You can then inspect, fix, or quarantine them. This pattern keeps your main tables clean while preserving visibility into data quality issues.
The Professional exam tests whether you can design end to end data quality systems. You need to know how to implement quality gates at each layer (Bronze, Silver, Gold), how to quarantine bad records for investigation, and how to monitor and alert on data quality violations. This is not optional. This is how mature data teams operate.
---
## Key Concepts
Lakeflow Expectations: expect, expect_or_drop, expect_or_fail
Expectations define quality constraints and are checked during pipeline execution. expect logs violations but does not stop the pipeline. expect_or_drop removes rows that fail the expectation (quarantine by dropping). expect_or_fail stops the pipeline if the expectation fails (strict mode). You can chain multiple expectations. The output is a quality report that tracks pass/fail rates over time.
Rescue Columns for Handling Parse Errors
When Auto Loader encounters unparseable data (schema mismatch, encoding errors, malformed JSON), it can rescue the raw data instead of failing. The _rescued_data column is a JSON string containing the raw bytes or unparseable fields. You then filter on _rescued_data IS NULL to get valid rows, and _rescued_data IS NOT NULL to find problematic rows. This is especially useful for evolutionary schemas where you do not want to fail on unknown fields.
Quarantine Tables and Bad Data Repositories
A quarantine table holds records that failed quality expectations or could not be parsed. Unlike discarding, quarantining preserves the bad data for investigation. You can later debug, fix, and replay. Quarantine tables should include metadata (original timestamp, reason for quarantine, attempt count) so you know when to investigate and how to route to a data steward.
Data Quality Metrics and Monitoring
Track metrics like pass rate, record counts, and quarantine rate. Use SQL alerts to notify when quality drops below a threshold. Log expectations to a metrics table for historical trending. This gives visibility into data quality over time and early warning of systemic issues.
---
## Code Examples
### Lakeflow Expectations
```python
import dlt
# Read from Auto Loader with rescue mode
df = spark.readStream \
  .format("cloudFiles") \
  .option("cloudFiles.format", "json") \
  .option("cloudFiles.schemaEvolutionMode", "rescue") \
  .load("/path/to/data/")
# Create a streaming table with expectations
@dlt.table(
  comment="Raw user events with quality expectations"
)
@dlt.expect("valid_json", "_rescued_data IS NULL")
@dlt.expect("non_null_user_id", "user_id IS NOT NULL")
@dlt.expect("valid_email", "email LIKE '%@%'")
def raw_events():
  return df
# Create another layer with stricter expectations (drop bad records)
@dlt.table(
  comment="Valid events only"
)
@dlt.expect_or_drop("email_not_empty", "email != ''")
@dlt.expect_or_drop("positive_amount", "amount > 0")
def valid_events():
  return dlt.read("raw_events").filter("_rescued_data IS NULL")
# Fail the pipeline if critical expectation fails
@dlt.table(
  comment="Processed events (fail on critical issues)"
)
@dlt.expect_or_fail("no_duplicates", "COUNT(*) == COUNT(DISTINCT event_id)")
def processed_events():
  return dlt.read("valid_events").select("user_id", "email", "amount", "timestamp")
```
### Quarantine Table Pattern
```python
import dlt
from pyspark.sql.functions import current_timestamp, lit
# Read raw data with rescue mode enabled
df_raw = spark.readStream \
  .format("cloudFiles") \
  .option("cloudFiles.format", "json") \
  .option("cloudFiles.schemaEvolutionMode", "rescue") \
  .load("/Volumes/my_catalog/my_schema/my_volume/events/")
# Separate good and bad records
df_good = df_raw.filter("_rescued_data IS NULL").drop("_rescued_data")
df_bad = df_raw.filter("_rescued_data IS NOT NULL").select(
  "_rescued_data",
  current_timestamp().alias("quarantine_timestamp"),
  lit("parse_error").alias("failure_reason")
)
# Append good records to main table
dlt.create_streaming_table(
  name="raw_events",
  comment="Valid events only"
)
dlt.append_or_replace_table(
  name="raw_events",
  spark_df=df_good
)
# Append bad records to quarantine table for investigation
dlt.create_streaming_table(
  name="quarantine_bad_events",
  comment="Events that could not be parsed"
)
dlt.append_or_replace_table(
  name="quarantine_bad_events",
  spark_df=df_bad
)
# Monitor quarantine table
@dlt.table(
  comment="Quarantine summary"
)
def quarantine_summary():
  return spark.sql("""
    SELECT 
      COUNT(*) as bad_record_count,
      DATE(quarantine_timestamp) as quarantine_date,
      failure_reason
    FROM quarantine_bad_events
    GROUP BY DATE(quarantine_timestamp), failure_reason
    ORDER BY quarantine_date DESC
  """)
```
### Using expect_or_drop with Expectations
```python
import dlt
from pyspark.sql.functions import col
# Read clean data
df_silver = dlt.read("raw_events").filter("_rescued_data IS NULL")
# Apply business logic rules and drop invalid rows
@dlt.table(
  comment="Gold layer with strict quality gates"
)
@dlt.expect_or_drop("positive_revenue", "revenue > 0")
@dlt.expect_or_drop("valid_date", "sale_date >= '2020-01-01'")
@dlt.expect_or_drop("email_format", "email REGEXP '^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$'")
def gold_customers():
  return df_silver.select(
    col("customer_id"),
    col("email"),
    col("revenue"),
    col("sale_date")
  )
# Monitor quality metrics
@dlt.table(
  comment="Quality metrics for gold layer"
)
def gold_quality_metrics():
  total = dlt.read("raw_events").filter("_rescued_data IS NULL").count()
  gold_count = dlt.read("gold_customers").count()
  return spark.createDataFrame([
    ("raw_events", total),
    ("gold_customers", gold_count),
    ("dropped_records", total - gold_count),
    ("pass_rate_pct", round(100.0 * gold_count / total, 2) if total > 0 else 0)
  ], ["layer", "count_or_metric"])
```
---
## Common Exam Scenarios
Scenario 1: Handling Schema Evolution with Quality Gates
Your upstream data source adds a new optional field. The old schema expects 10 fields, now there are 11. Your existing expectations fail. How do you evolve without breaking the pipeline?
Use rescue mode in Auto Loader to gracefully handle unknown fields. They go to _rescued_data. Your existing expectations still apply to known fields. Once you understand the new field, add it to the schema explicitly and create a new expectation for it. This decouples schema changes from quality gates.
Scenario 2: Quarantine Design for Investigation
Your Bronze layer has 100K records daily. 0.1% fail expectations. Should you quarantine them or drop them?
Quarantine them. Even if it is just 100 records daily, that is 36K records per year. They deserve investigation. Create a quarantine table with metadata (timestamp, reason, original data). Route to a data steward weekly to understand patterns and fix root causes. Dropping data without investigation is lazy.
Scenario 3: Critical vs Non Critical Expectations
Your Gold table needs user_id (critical), revenue (non critical), and email (critical). How do you design expectations?
Use expect_or_fail for user_id and email (stop the pipeline if missing). Use expect_or_drop for revenue (allow records without it). This way, critical business logic is protected but non critical missing values do not halt the pipeline. Design expectations proportionally to business impact.
---
## Key Takeaways
- Expectations are assertions about data quality. Use expect to log, expect_or_drop to filter, and expect_or_fail to stop.
- Quarantine bad data instead of dropping it. Metadata helps you investigate and improve upstream processes.
- Rescue mode in Auto Loader gracefully handles parse errors and schema evolution without failing the pipeline.
- Design expectations proportionally. Critical fields get expect_or_fail. Nice to have fields get expect_or_drop.
- Monitor quality metrics and quarantine rates. Use them to identify patterns and drive improvements.
---
## Gotchas and Tips
expect_or_drop can silently hide quality issues if you are not monitoring the drop rate. Always log expectations to a metrics table and alert if the drop rate changes unexpectedly.
Quarantine tables grow unbounded unless you have a retention policy. Set up a job to archive or delete old quarantine records after a retention period (e.g., 90 days).
expect_or_fail will halt your entire pipeline. Use it only for truly critical constraints. For most cases, prefer expect or expect_or_drop to keep the pipeline resilient.
---
## Links and Resources
- **[Lakeflow SDP Concepts](https://docs.databricks.com/aws/en/ldp/concepts)** — Using expectations and constraints to validate and quarantine bad data.
- **[Delta Best Practices](https://docs.databricks.com/aws/en/delta/best-practices)** — Data quality patterns and best practices for production Delta pipelines.