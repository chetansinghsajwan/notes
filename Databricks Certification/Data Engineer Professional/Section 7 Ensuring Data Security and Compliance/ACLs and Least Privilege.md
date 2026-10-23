## Topic Overview
Unity Catalog implements fine grained access control through a privilege hierarchy. At the top is the metastore, then catalogs, schemas, and tables. You assign privileges like SELECT, MODIFY, CREATE, and USAGE to principals at each level. This hierarchical model ensures data security while enabling collaboration.
The principle of least privilege means users get only the permissions they need, no more. Account admins set access at the workspace and metastore level. Workspace admins manage catalog and schema access. Data owners grant table level permissions. This distributed responsibility model works well in organizations of all sizes.
Unity Catalog permissions are enforced everywhere: SQL, Python, notebooks, and REST APIs. There is no way to bypass these controls. Combined with audit logging, this provides both security and compliance evidence.
---
## Key Concepts
Securable Objects
Securable objects form a hierarchy: metastore > catalog > schema > table > column > row. Each level has its own permissions. A user must have USAGE on the catalog, schema, and SELECT on the table to query it. Granular control at each level enables precise access patterns.
Privilege Types
SELECT allows reading data, INSERT/UPDATE/DELETE modify data, CREATE adds new objects, USAGE enables traversing the hierarchy, MODIFY changes properties. Different securable objects support different privileges. Tables support SELECT, MODIFY, but catalogs only support USAGE.
GRANT and REVOKE
GRANT privilege ON object TO principal assigns permissions. REVOKE removes them. Principals are users, groups, or service principals. Grants can include WITH GRANT OPTION allowing the recipient to grant to others. Revoking cascades through delegated grants.
Ownership and Admin
Object owners have full control including ability to grant/revoke. Account admins can change owners. Ownership allows delegation of responsibility without making someone an admin. Users inherit workspace level permissions, but explicit grants take precedence.
---
## Code Examples
### Granting Permissions
```sql
-- Grant SELECT to a user
GRANT SELECT ON TABLE my_catalog.my_schema.customers TO user@example.com;
-- Grant multiple privileges to a group
GRANT SELECT, MODIFY ON TABLE my_catalog.my_schema.orders TO data_engineers;
-- Grant USAGE on schema (required to access tables)
GRANT USAGE ON SCHEMA my_catalog.my_schema TO analysts;
-- Grant with delegation rights
GRANT SELECT ON TABLE my_catalog.my_schema.products 
  TO senior_engineer WITH GRANT OPTION;
-- Grant ownership
ALTER TABLE my_catalog.my_schema.transactions 
  OWNER TO data_engineering_team;
```
### Revoking and Checking Permissions
```sql
-- Revoke a privilege
REVOKE SELECT ON TABLE my_catalog.my_schema.customers FROM user@example.com;
-- Revoke all privileges
REVOKE ALL PRIVILEGES ON TABLE my_catalog.my_schema.orders FROM contractor_group;
-- Show grants on an object
SHOW GRANTS ON TABLE my_catalog.my_schema.customers;
-- Show grants for a principal
SHOW GRANTS ON my_catalog.my_schema.customers FOR user@example.com;
```
### Hierarchy Navigation
```sql
-- User needs USAGE on catalog to see schemas
GRANT USAGE ON CATALOG my_catalog TO analyst_group;
-- User needs USAGE on schema AND SELECT on table
GRANT USAGE ON SCHEMA my_catalog.my_schema TO analyst_group;
GRANT SELECT ON TABLE my_catalog.my_schema.products TO analyst_group;
-- Check what a user can access
-- This is implicit in the object hierarchy
-- Missing USAGE anywhere blocks access
```
---
## Common Exam Scenarios
Scenario 1: Multi Team Access Control
Finance team needs SELECT on financial tables, engineering team needs SELECT and MODIFY on product tables. Grant SELECT to finance_group on finance schema, SELECT and MODIFY to engineering_group on products schema. USAGE grants on the catalog itself allow both teams to navigate. Missing either privilege blocks access.
Scenario 2: Delegation Without Admin
A senior engineer needs to manage access to specific tables without becoming an admin. Use WITH GRANT OPTION to allow them to delegate SELECT to team members. They can grant and revoke but only SELECT, not MODIFY or administrative privileges.
Scenario 3: Service Principal Access
A machine learning pipeline needs read access to training data and write access to results. Grant SELECT on source tables and SELECT, INSERT on target tables to the service principal. The job runs with these specific privileges, enabling least privilege for automated processes.
---
## Key Takeaways
- Unity Catalog enforces hierarchical access control across all interfaces
- Users need privileges at every level: catalog USAGE, schema USAGE, table SELECT/MODIFY
- Grant privileges to groups for easier management at scale
- Use WITH GRANT OPTION to delegate responsibility without granting admin rights
---
## Gotchas and Tips
USAGE is Essential: A user missing USAGE at any level cannot see or access objects below. This is easy to forget when setting up new schemas or catalogs.
Group Membership: Use groups not individual users for easier management. Changes to group membership take effect immediately.
Privilege Specificity: Each privilege is distinct. Granting SELECT does not grant MODIFY. Be explicit about what each principal needs.
---
## Links and Resources
- **[Unity Catalog Access Control](https://docs.databricks.com/aws/en/data-governance/unity-catalog/access-control)** — Implementing ACLs and least privilege access on Unity Catalog securables.
- **[Unity Catalog Overview](https://docs.databricks.com/aws/en/data-governance/unity-catalog/)** — Understanding the three-level namespace and privilege inheritance model.