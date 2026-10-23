## Topic Overview
Anonymization and pseudonymization transform PII into forms that cannot identify individuals. Anonymization removes identifiability entirely through techniques like hashing or generalization. Pseudonymization replaces identifiers with random codes while maintaining record linkability. Both reduce risk and support compliance.
Hashing creates one way transformations ideal for one time comparisons. Tokenization replaces values with random codes while storing a mapping table separately. Suppression removes values entirely. Generalization groups exact values into ranges. Each technique has tradeoffs between security and utility.
K anonymity ensures at least k records are identical on quasi identifiers. This prevents individual reidentification. Differential privacy adds mathematical guarantees about privacy. Both are exam topics and important for regulatory compliance.
---
## Key Concepts
Hashing
One way function that transforms input deterministically. Same input always produces same output. Cannot reverse. Good for comparing values without revealing them. SHA2 is common for PII like emails. Salt adds randomness to resist dictionary attacks.
Tokenization
Replace actual values with random tokens. Maintain token mapping in secure vault. Allows reidentification if needed and linkability across records. More complex than hashing but reversible. Tokens should be cryptographically random.
Suppression and Generalization
Suppression removes values entirely, replacing with NULL. Generalization groups values, e.g., exact age to age ranges. Both reduce data utility. Good for non critical fields. Combine with other techniques for defense in depth.
K Anonymity
Ensures each combination of quasi identifiers appears at least k times. Prevents reidentification through record linkage. Higher k is more private but reduces utility. Exam often tests understanding of k anonymity tradeoffs.
---
## Code Examples
### Hashing Techniques
```sql
-- Hash with SHA2 (one way)
SELECT 
  SHA2(email, 256) as hashed_email,
  SHA2(phone, 256) as hashed_phone,
  name
FROM my_catalog.my_schema.customers;
-- Salted hash for security
SELECT 
  SHA2(CONCAT(email, 'my_salt_value'), 256) as salted_hash,
  customer_id
FROM my_catalog.my_schema.customers;
```
### Suppression and Generalization
```sql
-- Suppression: replace sensitive data with NULL
SELECT 
  NULL as ssn,
  NULL as credit_card,
  customer_name,
  city
FROM my_catalog.my_schema.customers;
-- Generalization: group exact values into ranges
SELECT 
  customer_id,
  CASE 
    WHEN age < 18 THEN '0-17'
    WHEN age < 65 THEN '18-64'
    ELSE '65+'
  END as age_group,
  LEFT(zip_code, 3) as zip_prefix,
  purchase_amount
FROM my_catalog.my_schema.customers;
```
### K Anonymity Check
```sql
-- Check k anonymity on quasi identifiers
SELECT 
  age_group, 
  city, 
  COUNT(*) as count
FROM (
  SELECT 
    CASE 
      WHEN age < 18 THEN '0-17'
      WHEN age < 65 THEN '18-64'
      ELSE '65+'
    END as age_group,
    city
  FROM my_catalog.my_schema.customers
)
GROUP BY age_group, city
HAVING count(*) < 5;  -- k=5, find groups with fewer than 5 records
```
---
## Links and Resources
- **[Row Filters and Column Masks](https://docs.databricks.com/aws/en/data-governance/unity-catalog/filters-and-masks)** — Using column masks for anonymization and pseudonymization of sensitive data.
- **[Unity Catalog Access Control](https://docs.databricks.com/aws/en/data-governance/unity-catalog/access-control)** — Controlling access to anonymized and pseudonymized datasets.