# `UNIQUE` Constraint

The `UNIQUE` constraint ensures that all values in a column are different.

> ##### Example
> 
> Creates a `UNIQUE` constraint on the `ID` column when the `Persons` table is created.
> 
> ```sql
> CREATE TABLE Persons (
>     ID int NOT NULL,
>     LastName varchar(255) NOT NULL,
>     FirstName varchar(255),
>     Age int,
>     UNIQUE (ID)
> );


> ##### Example
> 
> To create a `UNIQUE` constraint on the `ID` column when the table is already created.
> 
> ```sql
> ALTER TABLE Persons
> ADD UNIQUE (ID);
> ```

##### Drop a `UNIQUE` constraint

```
ALTER TABLE Persons
DROP INDEX UC_Person;
```