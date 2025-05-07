# `DELETE` Statement

The `DELETE` statement is used to delete existing records in a table.

##### Syntax

```sql
DELETE FROM table_name WHERE condition;
```

The `WHERE` clause specifies which records should be deleted.
If you omit the `WHERE` clause, all records in the table will be deleted.

##### Example

Deletes the customer `"Alfreds Futterkiste"` from the `"Customers"` table.

```sql
DELETE FROM Customers WHERE CustomerName='Alfreds Futterkiste';
```

##### Example

Deletes all records.

```sql
DELETE FROM Customers;
```
