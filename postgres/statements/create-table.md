# `CREATE TABLE` Statement

The `CREATE TABLE` statement is used to create a new table in a database.

##### Syntax

```sql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    column3 datatype,
   ....
);
```

##### Example

Creates a table called `Persons` that contains five columns: `PersonID,` `LastName,` `FirstName,` `Address,` and `City.`

```sql
CREATE TABLE Persons (
    PersonID int,
    LastName varchar(255),
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255)
);
```

### Create Table Using Another Table

A copy of an existing table can also be created using `CREATE TABLE`.

The new table gets the same column definitions. All columns or specific columns can be selected.

If you create a new table using an existing table, the new table will be filled with the existing values from the old table.

##### Syntax

```sql
CREATE TABLE new_table_name AS
    SELECT column1, column2,...
    FROM existing_table_name
    WHERE ....;
```

##### Example

Creates a new table called `TestTables` (which is a copy of the `Customers` table).

```sql
CREATE TABLE TestTable AS
SELECT customername, contactname
FROM customers;
```
