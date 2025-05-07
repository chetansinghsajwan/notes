# `BACKUP DATABASE` Statement

The `BACKUP DATABASE` statement is used in SQL Server to create a full back up of an existing SQL database.

##### Syntax

```sql
BACKUP DATABASE databasename
TO DISK = 'filepath';
```

##### Example

Creates a full back up of the existing database `testDB` to the D disk,

```sql
BACKUP DATABASE testDB
TO DISK = 'D:\backups\testDB.bak';
```

### The `BACKUP WITH DIFFERENTIAL` Statement

A differential back up only backs up the parts of the database that have changed since the last full database backup.

##### Syntax

```sql
BACKUP DATABASE databasename
TO DISK = 'filepath'
WITH DIFFERENTIAL;
```

##### Example

Creates a differential back up of the database `testDB`.

```sql
BACKUP DATABASE testDB
TO DISK = 'D:\backups\testDB.bak'
WITH DIFFERENTIAL;
```
