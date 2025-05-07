##### Create Database

The `CREATE DATABASE` statement is used to create a new SQL database.

> ##### Syntax
> 
> ```sql
> CREATE DATABASE database_name;
> ```

##### Drop Database

The `DROP DATABASE` statement is used to drop an existing SQL database.

> ##### Syntax
> 
> ```sql
> DROP DATABASE database_name_1, database_name_2, ...;
> ```

The `DROP DATABASE IF EXISTS` statement is used to drop an existing SQL database.

> ##### Syntax
> 
> ```sql
> DROP DATABASE IF EXISTS database_name_1, database_name_2, ...;
> ```

##### Selecting a database

The `USE DATABASE` statement is used to select a database from a list of databases available in the system.

> ##### Syntax
> 
> ```sql
> USE database_name;
> ```

##### Renaming a database

There used to be a simple RENAME DATABASE command in older versions of MySQL which was intended to rename database but RENAME DATABASE command has been removed from all newer versions to avoid security risks.

##### Rename Database using Dump and Reimport

If you are willing to rename a database name in MySQL, then a simple way is to dump the complete database in an SQL file and then re-import it into a new database.