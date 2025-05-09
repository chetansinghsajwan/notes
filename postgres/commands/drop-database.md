# `DROP DATABASE` Command

```
DROP DATABASE [ IF EXISTS ] name [ [ WITH ] ( option [, ...] ) ]

where option can be:

    FORCE
```

Only the owner of the database, or a superuser, can drop a database.

It removes the catalog entries for the database and deletes the directory containing the data.

This action cannot be undone.

You cannot execute this command while connected to the target database.

If anyone else is connected to the target database, this command will fail unless you use the `FORCE` option.

## References

- https://www.postgresql.org/docs/current/sql-dropdatabase.html
