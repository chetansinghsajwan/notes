# `ALTER ROLE` Command

```
ALTER ROLE _`role_specification`_ [ WITH ] _`option`_ [ ... ]

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

ALTER ROLE _`name`_ RENAME TO _`new_name`_

ALTER ROLE { _`role_specification`_ | ALL } [ IN DATABASE _`database_name`_ ] SET _`configuration_parameter`_ { TO | = } { _`value`_ | DEFAULT }
ALTER ROLE { _`role_specification`_ | ALL } [ IN DATABASE _`database_name`_ ] SET _`configuration_parameter`_ FROM CURRENT
ALTER ROLE { _`role_specification`_ | ALL } [ IN DATABASE _`database_name`_ ] RESET _`configuration_parameter`_
ALTER ROLE { _`role_specification`_ | ALL } [ IN DATABASE _`database_name`_ ] RESET ALL

where _`role_specification`_ can be:

    _`role_name`_
  | CURRENT_ROLE
  | CURRENT_USER
  | SESSION_USER
```