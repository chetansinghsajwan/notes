# `CREATE ROLE` Command

Adds a new role to a PostgreSQL database cluster.

```
CREATE ROLE _`name`_ [ [ WITH ] _`option`_ [ ... ] ]

where _`option`_ can be:

      SUPERUSER | NOSUPERUSER
    | CREATEDB | NOCREATEDB
    | CREATEROLE | NOCREATEROLE
    | INHERIT | NOINHERIT
    | LOGIN | NOLOGIN
    | REPLICATION | NOREPLICATION
    | BYPASSRLS | NOBYPASSRLS
    | CONNECTION LIMIT _`connlimit`_
    | [ ENCRYPTED ] PASSWORD '_`password`_' | PASSWORD NULL
    | VALID UNTIL '_`timestamp`_'
    | IN ROLE _`role_name`_ [, ...]
    | ROLE _`role_name`_ [, ...]
    | ADMIN _`role_name`_ [, ...]
    | SYSID _`uid`_
```

## References

- https://www.postgresql.org/docs/current/sql-createrole.html
