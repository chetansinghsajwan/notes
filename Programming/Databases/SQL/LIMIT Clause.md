The `LIMIT` clause is used to specify the number of records to return.
**Syntax**
```
SELECT column_name(s)
FROM table_name
WHERE condition
LIMIT number OFFSET number;
```
**Example**
Selects the first three records from the `"Customers"` table.
```
SELECT * FROM Customers
LIMIT 3;
```
**Example**
Selects records 4 - 6.
```
SELECT * FROM Customers
LIMIT 3 OFFSET 3;
```