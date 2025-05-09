# Postgres Tablespace

Tablespaces in PostgreSQL allow database administrators to define locations in the file system where the files representing database objects can be stored. Once created, a tablespace can be referred to by name when creating database objects.

By using tablespaces, an administrator can control the disk layout of a PostgreSQL installation. This is useful in at least two ways. First, if the partition or volume on which the cluster was initialized runs out of space and cannot be extended, a tablespace can be created on a different partition and used until the system can be reconfigured.

Second, tablespaces allow an administrator to use knowledge of the usage pattern of database objects to optimize performance. For example, an index which is very heavily used can be placed on a very fast, highly available disk, such as an expensive solid state device. At the same time a table storing archived data which is rarely used or not performance critical could be stored on a less expensive, slower disk system.

To create a tablespace use [`CREATE TABLESPACE`](postgres/commands/create-tablespace) command.
To alter a tablespace use [`ALTER TABLESPACE`](postgres/commands/alter-tablespace) command.
To drop a tablespace use [`DROP TABLESPACE`](postgres/commands/drop-tablespace) command.

To determine the set of existing tablespaces, examine the [`pg_tablespace`](https://www.postgresql.org/docs/current/catalog-pg-tablespace.html "51.56. pg_tablespace") system catalog, for example

```sql
SELECT spcname FROM pg_tablespace;
```

There is usually not much point in making more than one tablespace per logical file system, since you cannot control the location of individual files within a logical file system. However, PostgreSQL does not enforce any such limitation, and indeed it is not directly aware of the file system boundaries on your system. It just stores files in the directories you tell it to use.

Two tablespaces are automatically created when the database cluster is initialized. The `pg_global` tablespace is used only for shared system catalogs. The `pg_default` tablespace is the default tablespace of the `template1` and `template0` databases (and, therefore, will be the default tablespace for other databases as well, unless overridden by a `TABLESPACE` clause in `CREATE DATABASE`).

## References

- https://www.postgresql.org/docs/current/manage-ag-tablespaces.html
