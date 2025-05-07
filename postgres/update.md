# `UPDATE` Statement

The `UPDATE` statement is used to modify the existing records in a table.
**Syntax**

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

It is the `WHERE` clause that determines how many records will be updated.
If you do not specify `WHERE` clause, all records are selected.
**Example**
Updates a single record with `CustomerID = 1`.

```sql
UPDATE Customers
SET ContactName = 'Alfred Schmidt', City = 'Frankfurt'
WHERE CustomerID = 1;
```

**Example**

Updates the `PostalCode` to `00000` for all records where country is `"Mexico"`.

```sql
UPDATE Customers
SET PostalCode = 00000
WHERE Country = 'Mexico';
```

**Example**

Updates all records.

```sql
UPDATE Customers
SET PostalCode = 00000;
```
