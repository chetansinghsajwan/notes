# `CREATE ROLE` Command

Adds a new [role](postgres/role) to the database cluster.

```
CREATE ROLE <name> [ [ WITH ] <option> [ ... ] ]

where <option> can be:

      SUPERUSER | NOSUPERUSER
    | CREATEDB | NOCREATEDB
    | CREATEROLE | NOCREATEROLE
    | INHERIT | NOINHERIT
    | LOGIN | NOLOGIN
    | REPLICATION | NOREPLICATION
    | BYPASSRLS | NOBYPASSRLS
    | CONNECTION LIMIT connlimit
    | [ ENCRYPTED ] PASSWORD 'password' | PASSWORD NULL
    | VALID UNTIL '_`timestamp`_'
    | IN ROLE _`role_name`_ [, ...]
    | ROLE _`role_name`_ [, ...]
    | ADMIN _`role_name`_ [, ...]
    | SYSID _`uid`_
```

## References

- https://www.postgresql.org/docs/current/sql-createrole.html
