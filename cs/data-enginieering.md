
# Database vs Datawarhouse vs Datalake

## Datawarehouse

A data warehouse is a system that stores highly structured information from various sources. Data warehouses typically store current and historical data from one or more systems. The goal of using a data warehouse is to combine disparate data sources in order to analyze the data, look for insights, and create business intelligence (BI) in the form of reports and dashboards.

You might be wondering, "Is a data warehouse a database?" Yes, a data warehouse is a giant database that is optimized for analytics.

Complex queries are very difficult to run without a temporary pause of database update operations. A frequently paused transactional database will inevitably lead to data errors and gaps. Therefore a data warehouse serves as a separate platform for aggregation across multiple sources and then for analytics tasks across those diverse sources. This separation of roles allows databases to remain focused on purely transactional jobs without interruption.

## References

- https://www.mongodb.com/resources/basics/databases/data-lake-vs-data-warehouse-vs-database
- https://www.snowflake.com/data-cloud-glossary/data-warehousing/
