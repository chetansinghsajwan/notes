# `ALTER TABLE` Statement

The `ALTER TABLE` statement is used to alter the structure of an existing table.

##### ADD Column

Adds a column in the table.

##### Syntax

```sql
ALTER TABLE table_name
ADD column_name datatype;
```

##### Example

Adds an `Email` column to the `Customers` table.

```sql
ALTER TABLE Customers
ADD Email varchar(255);
```

##### DROP Column