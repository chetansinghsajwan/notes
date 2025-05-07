# `WHERE` Clause

The `WHERE` clause is used to filter records. It is used to extract only those records that fulfill a specified condition.

**Syntax**

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

**Example**

The following SQL statement selects all the customers from "Mexico":

```sql
SELECT * FROM Customers
WHERE Country = 'Mexico';
```
