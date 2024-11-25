# `LIMIT` Clause

The `LIMIT` clause is used to specify the number of records to return.

> ##### Syntax
> 
> ```sql
> SELECT column_name(s)
> FROM table_name
> WHERE condition
> LIMIT number OFFSET number;
> ```

> ##### Example
> 
> Selects the first three records from the `"Customers"` table.
> 
> ```sql
> SELECT * FROM Customers
> LIMIT 3;
> ```

> ##### Example
> 
> Selects records 4 - 6.
> 
> ```sql
> SELECT * FROM Customers
> LIMIT 3 OFFSET 3;
> ```