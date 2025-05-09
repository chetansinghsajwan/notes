# `ALTER DATABASE` Command

This command changes the attributes of the database.

---

```
ALTER DATABASE name [ [ WITH ] option [ ... ] ]

where option can be:

    ALLOW_CONNECTIONS allowconn
    CONNECTION LIMIT connlimit
    IS_TEMPLATE istemplate
```

Changes certain per-database settings. (See below for details.) Only the database owner or a superuser can change these settings.

_`allowconn`_

If false then no one can connect to this database.

_`connlimit`_

How many concurrent connections can be made to this database. -1 means no limit.

_`istemplate`_

If true, then this database can be cloned by any user with `CREATEDB` privileges; if false, then only superusers or the owner of the database can clone it.

---

```
ALTER DATABASE name RENAME TO new_name
```

Renames the database.

---

```
ALTER DATABASE name OWNER TO { new_owner | CURRENT_ROLE | CURRENT_USER | SESSION_USER }
```

Changes the owner of the database.

---

```
ALTER DATABASE name SET TABLESPACE new_tablespace
```

Changes the default tablespace of the database. Only the database owner or a superuser can do this; you must also have create privilege for the new tablespace. This command physically moves any tables or indexes in the database's old default tablespace to the new tablespace. The new default tablespace must be empty for this database, and no one can be connected to the database. Tables and indexes in non-default tablespaces are unaffected.

---

```
ALTER DATABASE name REFRESH COLLATION VERSION

ALTER DATABASE name SET configuration_parameter { TO | = } { value | DEFAULT }
ALTER DATABASE name SET configuration_parameter FROM CURRENT
ALTER DATABASE name RESET configuration_parameter
ALTER DATABASE name RESET ALL
```

## References

- https://www.postgresql.org/docs/current/sql-alterdatabase.html
