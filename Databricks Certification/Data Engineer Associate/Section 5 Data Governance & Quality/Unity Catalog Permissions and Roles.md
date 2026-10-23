## Topic Overview
**Unity Catalog (UC)** is the centralized governance layer in Databricks that provides fine grained access control for all data assets. It enforces a three level namespace (`catalog.schema.table`) and allows administrators to manage who can access what data using standard SQL GRANT and REVOKE statements.
Permissions in UC follow a hierarchy. Users need USE CATALOG on the catalog and USE SCHEMA on the schema before they can SELECT from a table. This layered approach means you can control access at different levels of granularity. The system also supports ownership, where object owners can manage permissions on the objects they own.
For the exam, know the permission model inside out: what each privilege does, how inheritance works, the difference between admin roles, and the best practice of using groups with least privilege.
---
## Key Concepts
**Three level namespace**
Unity Catalog organizes data as catalog.schema.table_or_object. A metastore sits above catalogs as the top level container. Within a schema, you can have tables, views, volumes, functions, and models. This hierarchy gives you clear organizational boundaries and maps naturally to environments (dev_catalog, prod_catalog) or business domains (finance_catalog, marketing_catalog).
**Securable objects hierarchy**
The hierarchy from top to bottom: Metastore → Catalog → Schema → Table / View / Volume / Function / Model. Each level is a securable object that can have permissions granted on it. Permissions flow downward through inheritance but access requires explicit privileges at each namespace level (USE CATALOG + USE SCHEMA + SELECT).
**Privileges**
Key privileges include: USE CATALOG (access a catalog), USE SCHEMA (access a schema), SELECT (read from tables/views), MODIFY (insert/update/delete), CREATE TABLE, CREATE SCHEMA, CREATE CATALOG, EXECUTE (run functions), READ VOLUME, WRITE VOLUME, and ALL PRIVILEGES. The USE privileges are namespace access gates. Without USE CATALOG on a catalog, you cannot access anything inside it, even if you have SELECT on a table within.
**GRANT and REVOKE**
Access is managed through SQL statements: GRANT privilege ON securable TO principal and REVOKE privilege ON securable FROM principal. Principals can be users (individual accounts), groups (recommended), or service principals (for automated processes). You can also use SHOW GRANTS ON securable to see current permissions and SHOW GRANTS TO principal to see what a user or group can access.
**Permission inheritance**
Granting a privilege on a parent applies it to all current and future children. For example, GRANT SELECT ON CATALOG my_catalog TO analysts gives the analysts group SELECT on every table in every schema within that catalog, including tables created later. However, users still need USE CATALOG and USE SCHEMA to navigate to the tables. Inheritance saves you from granting permissions table by table.
**Ownership and admin roles**
Every securable object has an owner. The creator of an object becomes its owner by default. Owners have full control over their objects, including granting permissions to others. Ownership can be transferred with ALTER securable OWNER TO principal. Admin roles include: Account admin (manages the Databricks account), Workspace admin (manages workspace settings), and Metastore admin (top level UC admin with full control over all data objects in the metastore).
---
## Code Examples
### Granting permissions
```sql
-- Grant access to a catalog
GRANT USE CATALOG ON CATALOG prod_catalog TO data_engineers;
-- Grant access to a schema
GRANT USE SCHEMA ON SCHEMA prod_catalog.sales TO data_engineers;
-- Grant read access to all tables in a schema
GRANT SELECT ON SCHEMA prod_catalog.sales TO data_analysts;
-- Grant write access to a specific table
GRANT MODIFY ON TABLE prod_catalog.sales.orders TO etl_service_principal;
-- Grant ability to create tables
GRANT CREATE TABLE ON SCHEMA prod_catalog.sales TO data_engineers;
```
### Viewing and revoking permissions
```sql
-- Show grants on a table
SHOW GRANTS ON TABLE prod_catalog.sales.orders;
-- Show all grants for a principal
SHOW GRANTS TO `data_engineers`;
-- Revoke access
REVOKE SELECT ON SCHEMA prod_catalog.sales FROM former_team;
-- Transfer ownership
ALTER SCHEMA prod_catalog.sales OWNER TO `platform_team`;
```
---
## Common Exam Scenarios
**Scenario 1: User cannot access a table despite having SELECT**
An analyst has been granted SELECT on a specific table but gets an access denied error when querying it. The most likely cause is missing USE CATALOG or USE SCHEMA privileges. In Unity Catalog, you need all three: USE CATALOG on the catalog, USE SCHEMA on the schema, and SELECT on the table. Granting SELECT alone is not enough.
**Scenario 2: Granting access to all current and future tables**
A team lead wants analysts to be able to read all tables in a schema, including any new tables added later. The correct approach is to GRANT SELECT ON SCHEMA, which applies to all current and future tables within that schema via inheritance. Granting SELECT on individual tables would not cover future tables.
**Scenario 3: Least privilege for a service principal**
An ETL pipeline runs as a service principal and needs to read from bronze tables and write to silver tables. The minimum privileges are: USE CATALOG, USE SCHEMA on both schemas, SELECT on the bronze schema, and MODIFY plus CREATE TABLE on the silver schema. Granting ALL PRIVILEGES would work but violates the principle of least privilege.
---
## Key Takeaways
- Unity Catalog uses a three level namespace (catalog.schema.table) with permissions at each level.
- Users need USE CATALOG + USE SCHEMA + SELECT (or MODIFY) to access tables. Missing any one of these blocks access.
- Permissions inherit downward. Granting SELECT on a schema applies to all current and future tables in it.
- Use groups for access management, not individual users. Follow the principle of least privilege.
- Object owners have full control and can grant permissions to others. Ownership is transferable with ALTER ... OWNER TO.
---
## Gotchas and Tips
**USE CATALOG and USE SCHEMA are not optional.** This is the most common exam trap for UC permissions. Even with SELECT granted on a table, the user will get access denied without USE CATALOG and USE SCHEMA. Always check all three levels.
**ALL PRIVILEGES is rarely the right answer.** On the exam, if an option uses ALL PRIVILEGES, it is usually a distractor unless the question specifically asks for full administrative access. Prefer specific, minimal grants.
**Metastore admin vs Catalog owner.** The metastore admin has control over everything in the metastore. A catalog owner only has control within their catalog. Know the scope of each role. Metastore admins can also transfer ownership of any object.
---
## Links and Resources
- **[Unity Catalog Access Control](https://docs.databricks.com/aws/en/data-governance/unity-catalog/access-control)** — Managing privileges on securable objects in Unity Catalog.
- **[Unity Catalog Overview](https://docs.databricks.com/aws/en/data-governance/unity-catalog/)** — Comprehensive guide to Unity Catalog governance model, roles, and architecture.