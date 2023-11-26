The `SELECT` statement is used to select data from a database.
The data returned is stored in a result table, called the result-set.
**Syntax**
```
SELECT column1, column2, ...
FROM table_name;
```
The `SELECT *` statement selects all columns.
**Example**
```
SELECT * FROM table_name;
```
The `SELECT DISTINCT` statement is used to return only distinct (different) values.
**Example**
```
SELECT DISTINCT column1, column2, ...
FROM table_name;
```