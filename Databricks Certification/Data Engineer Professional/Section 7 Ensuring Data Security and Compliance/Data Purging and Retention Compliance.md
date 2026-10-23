## Topic Overview
Data purging removes data that is no longer needed or has exceeded its retention period. GDPR right to be forgotten and similar regulations require demonstrating data deletion. Tombstone records mark deleted data without removing it physically, providing audit trails. Lakeflow pipelines automate deletion for compliance.
VACUUM permanently removes deleted data from Delta tables. Combine with deletion workflows to meet compliance requirements. Test deletion logic on non production first. Document all deletions for audits, especially GDPR requests.
Retention policies define how long different data classes are kept. Implement at the table level using lifecycle policies. Automate purging with scheduled jobs to maintain compliance without manual effort. Immutable tables protect audit logs from tampering.
---
## Key Concepts
GDPR Right to Be Forgotten
Individuals can request erasure of their data. Organizations have 30 days to comply. Must delete across all systems. VAULT command with PURGE ensures data is unrecoverable. Log deletions with timestamps and requester for audit.
Tombstone Records
Soft delete that marks records as deleted without removing them. Store deletion timestamp and reason. Enables audit trails and recovery if needed. Use is_deleted flag in filter for normal queries.
Retention Policies
Define minimum and maximum retention periods. Minimum ensures data is available when needed. Maximum ensures deletion after legal holds expire. Enforce programmatically with automated purging.
Audit Trail Immutability
Audit logs must be immutable to prevent tampering. Store separately from operational data. Use managed tables or external append only stores. Never purge audit records within retention window.
---
## Code Examples
### Hard Delete with VACUUM
```sql
-- Delete specific records and purge immediately
DELETE FROM my_catalog.my_schema.customers
WHERE customer_id = 12345;
VACUUM my_catalog.my_schema.customers
RETAIN 0 HOURS;  -- Immediately remove deleted data
-- Drop entire table and purge
DROP TABLE my_catalog.my_schema.customers PURGE;
```
### Soft Delete with Tombstones
```sql
-- Add tombstone fields
ALTER TABLE my_catalog.my_schema.customers
ADD COLUMN is_deleted BOOLEAN DEFAULT FALSE,
ADD COLUMN deleted_at TIMESTAMP,
ADD COLUMN deletion_reason STRING;
-- Soft delete with audit trail
UPDATE my_catalog.my_schema.customers
SET is_deleted = TRUE, deleted_at = current_timestamp(), deletion_reason = 'GDPR request'
WHERE customer_id = 12345;
-- Query excludes deleted records
SELECT * FROM my_catalog.my_schema.customers
WHERE is_deleted = FALSE;
```
### Automated Retention Purging
```python
# Lakeflow pipeline for retention based purging
from databricks.sdk.python.lakehouse import *
@dlt.view
def expired_records():
    retention_days = 90
    return spark.sql(f"""
        SELECT customer_id FROM my_catalog.my_schema.customers
        WHERE created_at < date_sub(current_date(), {retention_days})
        AND retention_type = 'transient'
    """)
@dlt.table
def purge_expired():
    spark.sql("""
        DELETE FROM my_catalog.my_schema.customers
        WHERE customer_id IN (SELECT customer_id FROM expired_records)
    """)
    return spark.sql(f"""SELECT COUNT(*) as deleted FROM {dlt.table} LIMIT 1""")
```
---
## Common Exam Scenarios
Scenario 1: GDPR Deletion Request
Customer requests deletion of their data. You must delete across all systems within 30 days. Delete from production tables with PURGE, then from backups after retention expires. Log the deletion request and completion timestamp for compliance proof.
Scenario 2: Retention Window with Tombstones
Financial records must be kept for 7 years per regulation. Mark deleted with tombstone, not hard delete. After 7 years, hard delete with VACUUM. Tombstones provide audit trail during retention window.
Scenario 3: Automated Compliance Purging
Session logs older than 30 days have no compliance value. Schedule daily job to identify and delete expired sessions. VACUUM immediately to free storage. Alert if deletion fails.
---
## Key Takeaways
- Hard delete with VACUUM is unrecoverable. Use for GDPR requests and expired data past retention.
- Tombstone records preserve audit trails during retention windows without permanently removing data
- Automate retention purging with scheduled jobs to ensure compliance without manual effort
- Keep audit logs immutable and separate from operational data
---
## Gotchas and Tips
PURGE is Irreversible: Once you PURGE, there is no recovery. Double check your WHERE clause on non production first.
Deletion Auditing: Log all deletion requests with requester, reason, timestamp, and row count. GDPR audits require this documentation.
Backup Strategy: Backups may retain deleted data briefly. Ensure backup retention aligns with purge requirements.
---
## Links and Resources
- **[Deletion Vectors](https://docs.databricks.com/aws/en/delta/deletion-vectors)** — Efficient row-level deletion for GDPR/CCPA data purging compliance.
- **[Delta Best Practices](https://docs.databricks.com/aws/en/delta/best-practices)** — Retention policies and VACUUM strategies for data lifecycle management.