# `PRIMARY KEY` Constraint

The `PRIMARY KEY` constraint uniquely identifies each record in a table.
Primary keys must contain unique values, and cannot contain null values.
A table can have only one primary key.

The primary key can consist of single or multiple columns (fields).

##### Creating table with primary key

> ##### Syntax
> 
> ```sql
> CREATE TABLE Persons (
>     ID int NOT NULL,
>     LastName varchar(255) NOT NULL,
>     FirstName varchar(255),
>     Age int,
>     PRIMARY KEY (ID)
> );
> ```

##### Adding primary key on existing table

> ##### Syntax
> 
> ```sql
> ALTER TABLE table_name
> ADD PRIMARY KEY (column_name);
> ```

To allow naming of a `PRIMARY KEY` constraint, and for defining a `PRIMARY KEY` constraint on multiple columns.

> ##### Syntax
> 
> ```sql
> ALTER TABLE table_name
> ADD CONSTRAINT constraint_name PRIMARY KEY (column1, column2, ...);
> ```

##### Dropping constraint

> ##### Syntax
> 
> ```sql
> ALTER TABLE table_name
> DROP PRIMARY KEY;
> ```

Using constraint name.

> ##### Syntax
> 
> ```sql
> ALTER TABLE table_name
> DROP CONSTRAINT constraint_name;
> ```