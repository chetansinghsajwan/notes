# `CREATE ROLE` Command

Adds a new role to a PostgreSQL database cluster. A role is an entity that can own database objects and have database privileges; a role can be considered a “user”, a “group”, or both depending on how it is used.

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
