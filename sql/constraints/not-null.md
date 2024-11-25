# `NOT NULL` Constraint

By default, a column can hold NULL values.

The `NOT NULL` constraint enforces a column to NOT accept NULL values.
This enforces a field to always contain a value, which means that you cannot insert a new record, or update a record without adding a value to this field.

> ##### Example
> 
> Ensures that the `ID`, `LastName`, and `FirstName` columns will not accept `NULL` values when the `Persons` table is created.
> 
> ```sql
> CREATE TABLE Persons (
>     ID int NOT NULL,
>     LastName varchar(255) NOT NULL,
>     FirstName varchar(255) NOT NULL,
>     Age int
> );
> ```

> ##### Example
> 
> To create a `NOT NULL` constraint on the `Age` column when the `Persons` table is already created.
> 
> ```sql
> ALTER TABLE Persons
> MODIFY COLUMN Age int NOT NULL;
> ```