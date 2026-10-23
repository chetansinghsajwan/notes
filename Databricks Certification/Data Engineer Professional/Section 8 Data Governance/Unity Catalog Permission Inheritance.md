## Topic Overview
Understanding the Unity Catalog permission hierarchy is critical for designing secure, scalable governance in the Lakehouse. Permissions flow downward from the metastore level to schemas, tables, and columns. If you grant a user USAGE on a catalog, they automatically inherit USAGE on all schemas within it, even if you never explicitly grant schema level permissions. This hierarchical model makes governance easier but also means a single permission grant can have cascading effects.
The Professional exam tests your ability to troubleshoot permission issues, design least privilege access models, and explain how ownership, inheritance, and explicit permissions interact. You need to know when to use table level grants versus catalog level grants, and how DENY permissions work in UC.
## Key Concepts
**Hierarchical Permission Model (Metastore > Catalog > Schema > Table/View)**
The UC metastore is the top level container. Catalogs live in the metastore, schemas live in catalogs, and tables/views live in schemas. Permissions granted at a parent level propagate to children automatically. If you grant SELECT on a catalog, users can SELECT from all tables in all schemas within that catalog without additional grants.
**USAGE Privilege and Its Role in Permission Chains**
USAGE is a special privilege. To access any table in a schema, a user must have USAGE on both the parent catalog and the parent schema. USAGE does not grant the right to SELECT or manipulate data; it is just a gate. If a user lacks USAGE on the catalog, they cannot see or access any schema within it, even if they are granted SELECT directly on a table. USAGE is purely about traversal through the hierarchy.
**Inheritance vs. Explicit Grants and Permission Checking Order**
UC evaluates permissions from the most specific to the least specific. A table level grant overrides a schema level grant. If a user has DENY SELECT at the catalog level but GRANT SELECT at the table level, the table level grant wins. The permission checking order is: object level (most specific) > parent schema level > parent catalog level > metastore level (least specific).
**Owner Privileges and Implicit Rights**
The owner of a table or schema has full rights to that object and can grant and revoke permissions without restrictions. Ownership is independent of explicit GRANT statements. If you are the owner of a table, you can always SELECT, INSERT, UPDATE, DELETE, and ALTER that table, even if no explicit grant exists. You can also change ownership and transfer permissions.
## Code Examples
### Granting Permissions at Different Levels
```sql
-- Grant USAGE on catalog (must have this to access any schema)
GRANT USAGE ON CATALOG my_catalog TO `data_team@company.com`;
-- Grant USAGE on schema (required to access tables in schema)
GRANT USAGE ON SCHEMA my_catalog.my_schema TO `data_team@company.com`;
-- Grant SELECT on all tables in schema
GRANT SELECT ON SCHEMA my_catalog.my_schema TO `data_team@company.com`;
-- Grant SELECT on specific table only
GRANT SELECT ON TABLE my_catalog.my_schema.customers TO `data_team@company.com`;
-- Grant multiple privileges on table
GRANT SELECT, INSERT, UPDATE ON TABLE my_catalog.my_schema.orders 
  TO `analytics@company.com`;
-- Grant privileges to a group (members inherit)
GRANT SELECT ON CATALOG my_catalog TO `group_analysts`;
```
### Testing Permission Inheritance
```sql
-- View all permissions on a table
SHOW GRANT ON TABLE my_catalog.my_schema.customers;
-- View all permissions on a schema
SHOW GRANT ON SCHEMA my_catalog.my_schema;
-- View all permissions on a catalog
SHOW GRANT ON CATALOG my_catalog;
-- Check effective permissions (what a principal can actually do)
SHOW GRANT `user@company.com` ON SCHEMA my_catalog.my_schema;
-- List all principals with access to a catalog
SHOW GRANT ON CATALOG my_catalog;
```
### Managing Ownership and Explicit Denies
```sql
-- Change table owner
ALTER TABLE my_catalog.my_schema.customers
OWNER TO `new_owner@company.com`;
-- Explicitly DENY a privilege (overrides parent grants)
DENY SELECT ON TABLE my_catalog.my_schema.sensitive_data 
  TO `external_user@company.com`;
-- REVOKE a privilege
REVOKE SELECT ON TABLE my_catalog.my_schema.customers
FROM `user@company.com`;
-- Change schema owner
ALTER SCHEMA my_catalog.my_schema
OWNER TO `schema_admin@company.com`;
```
## Common Exam Scenarios
**A user reports they cannot SELECT from a table even though they have SELECT on the schema. You check SHOW GRANT and see the user has SELECT on the schema but not the table. What is likely happening?**
The user likely lacks USAGE on the parent catalog. While SELECT on a schema grants read access to tables, the user must also have USAGE on the catalog to traverse the hierarchy. Ask them to check: do they have USAGE on the catalog? If not, grant it. The permission checking order requires USAGE at every level before data access privileges take effect.
**You granted SELECT on a catalog to all analysts, but a new sensitive table was added that should only be readable by a specific team. The table is inheriting the catalog level SELECT grant. How do you restrict access without revoking the catalog grant?**
Use an explicit DENY on the sensitive table for the broader group, then GRANT SELECT to just the specific team. Explicit object level permissions override parent permissions. The DENY on the table will prevent the inherited catalog level SELECT from taking effect for users in the broader group, while the specific team members receive a direct GRANT.
## Key Takeaways
- USAGE is required at every level in the hierarchy. A user needs USAGE on the catalog AND schema to access a table.
- Permissions flow downward from metastore to catalog to schema to table.
- More specific grants always win. Table level permissions override schema level permissions.
- Owners have implicit rights and can revoke permissions even without explicit grants.
## Gotchas and Tips
Granting SELECT on a schema does not grant USAGE on the parent catalog. You must grant both USAGE on the catalog and SELECT on the schema for a user to read tables.
DENY does not cascade downward. If you DENY a privilege on a schema, child tables can still be accessed if a lower level GRANT exists. Permissions are evaluated from most specific to least specific.
---
## Links and Resources
- **[Unity Catalog Access Control](https://docs.databricks.com/aws/en/data-governance/unity-catalog/access-control)** — Understanding permission inheritance across metastore, catalog, schema, and table levels.
- **[Unity Catalog Overview](https://docs.databricks.com/aws/en/data-governance/unity-catalog/)** — Three-level namespace hierarchy and governance model.