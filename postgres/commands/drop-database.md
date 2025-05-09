# `DROP DATABASE` Command

```
DROP DATABASE [ IF EXISTS ] name [ [ WITH ] ( option [, ...] ) ]

where option can be:

    FORCE
```

Only the owner of the database, or a superuser, can drop a database.

Dropping a database removes all objects that were contained within the database.

This action cannot be undone.

You cannot execute this command while connected to the target database.

If anyone else is connected to the target database, this command will fail unless you use the `FORCE` option.

## References

- https://www.postgresql.org/docs/current/manage-ag-dropdb.html
