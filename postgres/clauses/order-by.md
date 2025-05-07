# `ORDER BY` Clause

The `ORDER BY` keyword is used to sort the result-set in ascending or descending order.

The `ORDER BY` keyword sorts the records in ascending order by default. To sort the records in descending order, use the `DESC` keyword.

> ##### Syntax
> 
> ```sql
> SELECT column1, column2, ...
> FROM table_name
> ORDER BY column1, column2, ... ASC|DESC;
> ```

> ##### Example
> 
> Selects all customers from the `Customers` table, sorted by the `Country` column in ascending order.
> 
> ```sql
> SELECT * FROM Customers
> ORDER BY Country;
> 
> -- OR --
> 
> SELECT * FROM Customers
> ORDER BY Country ASC;
> ```

> ##### Example
> 
> Selects all customers from the `Customers` table, sorted by the `Country` and the `CustomerName` column. This means that it orders by Country, but if some rows have the same `Country`, it orders them by `CustomerName`.
> 
> ```sql
> SELECT * FROM Customers
> ORDER BY Country, CustomerName;
> ```

> ##### Example
> 
> Selects all customers from the `Customers` table, sorted ascending by the `Country` and descending by the `CustomerName` column.
> 
> ```sql
> SELECT * FROM Customers
> ORDER BY Country ASC, CustomerName DESC;
> ```