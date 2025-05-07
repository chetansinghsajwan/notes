# `INSERT INTO` Statement

The `INSERT INTO` statement is used to insert new records in a table.

##### Syntax

It is possible to write the `INSERT INTO` statement in two ways

1. Specify both the column names and the values to be inserted

    ```sql
    INSERT INTO table_name (column1, column2, column3, ...)
    VALUES (value1, value2, value3, ...);
    ```

2. If you are adding values for all the columns of the table, you do not need to specify the column names in the SQL query.

    ```sql
    INSERT INTO table_name
    VALUES (value1, value2, value3, ...);
    ```

##### Example

```sql
INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
VALUES ('Cardinal', 'Tom B. Erichsen', 'Skagen 21', 'Stavanger', '4006', 'Norway');
```
