## Topic Overview
Row and column level security enable fine grained access control without duplicating data. Row filters restrict which rows a user can see based on SQL expressions. Column masks hide sensitive values while showing the column structure. Together, they provide defense in depth for sensitive data.
Row filters work by adding automatic WHERE clauses to queries. If a user lacks permission to see rows where status=deleted, those rows vanish from all queries they run. Column masks replace sensitive values with NULL or hashed versions dynamically. Neither duplicates data, making them efficient and maintainable.
Performance implications exist at scale. Filters and masks execute for every query, so complex expressions on large tables can slow things down. Test performance before applying to high traffic tables.
---
## Key Concepts
Row Level Security
Row filters apply SQL predicates automatically based on user or group. Users see only rows matching the filter. Multiple filters on the same table combine with OR. Filters work across all query interfaces and cannot be bypassed.
Column Level Masking
Masks replace sensitive values dynamically. Options include hiding entirely, showing only first N characters, hashing with DIGEST, or custom SQL expressions. Masks apply to all accessors, but owners and admins see unmasked data.
Owner and Admin Exceptions
Table owners and admins automatically bypass filters and masks. This allows them to audit and manage data. Service accounts should rarely be admins due to this bypass.
Performance Considerations
Every query evaluates filters and masks, adding CPU and I/O. Complex expressions degrade performance. Indexed columns in filters can help. Test at scale before deploying.
---
## Code Examples
### Creating Row Filters
```sql
-- Row filter that shows only rows where department = user's department
CREATE OR REPLACE ROW FILTER row_filter_dept
ON my_catalog.my_schema.employees
AS (department = current_user_name());
-- Apply filter to analyst group
ALTER ROW FILTER row_filter_dept
GRANT (APPLY) ON my_catalog.my_schema.employees TO analysts;
-- Row filter using function
CREATE OR REPLACE ROW FILTER row_filter_by_region
ON my_catalog.my_schema.orders
AS (region IN (SELECT allowed_regions FROM my_catalog.my_schema.user_regions
              WHERE user_id = current_user()));
```
### Creating Column Masks
```sql
-- Hide entire column (show NULL)
CREATE OR REPLACE COLUMN MASK mask_salary_hidden
ON my_catalog.my_schema.employees COLUMN salary
RETURN NULL;
-- Show only first 4 digits of credit card
CREATE OR REPLACE COLUMN MASK mask_cc_partial
ON my_catalog.my_schema.payments COLUMN credit_card_number
RETURN CONCAT('****-****-****-', RIGHT(credit_card_number, 4));
-- Hash sensitive email
CREATE OR REPLACE COLUMN MASK mask_email_hash
ON my_catalog.my_schema.customers COLUMN email
RETURN REGEXP_REPLACE(email, '@.*', '@example.com');
-- Apply masks to group
ALTER COLUMN MASK mask_salary_hidden
GRANT (APPLY) ON my_catalog.my_schema.employees COLUMN salary TO non_hr_staff;
```
---
## Common Exam Scenarios
Scenario 1: Department Specific Data Access
HR employees should see all employee records. Managers should see only their department. Apply a row filter WHERE department IN (SELECT department FROM user_dept_mapping) to restrict managers.
Scenario 2: PII Column Masking
Finance analysts need to see customer amounts but not credit card numbers. Apply a mask on credit_card_number showing only last 4 digits. HR can see unmasked values. Admins always see everything.
Scenario 3: Compliance with Row Filters
Deleted user records should be invisible to most users. Create a row filter WHERE is_deleted = false. Only data governance team with DELETE privilege can see deleted records for audits.
---
## Key Takeaways
- Row filters add WHERE clauses automatically, restricting visible rows
- Column masks replace values dynamically, hiding sensitive data without duplication
- Owners and admins bypass filters and masks, enabling auditing
- Test performance impact on large tables before deploying at scale
---
## Gotchas and Tips
Admin Bypass: Admins see unfiltered data even if they do not need it. Minimize admin privileges to limit exposure.
Filter Complexity: Complex filter logic that joins to other tables can be slow. Denormalize lookup data into the main table if needed.
Mask Reversibility: Masks are not reversible. Once hashed or partially masked, original values are lost. Plan for audit trail separately.
---
## Links and Resources
- **[Row Filters and Column Masks](https://docs.databricks.com/aws/en/data-governance/unity-catalog/filters-and-masks)** — Applying fine-grained access control at the row and column level using SQL functions.