# Roles

PostgreSQL manages database access permissions using the concept of _roles_. A role can be thought of as either a database user, or a group of database users, depending on how the role is set up. Roles can own database objects (for example, tables and functions) and can assign privileges on those objects to other roles to control who has access to which objects. Furthermore, it is possible to grant _membership_ in a role to another role, thus allowing the member role to use privileges assigned to another role.

The concept of roles subsumes the concepts of “users” and “groups”. In PostgreSQL versions before 8.1, users and groups were distinct kinds of entities, but now there are only roles. Any role can act as a user, a group, or both.

- To create a role, use [`create role`](postgres/commands/createrole) sql command.
- To alter a role, use [`alter role`](postgres/commands/alterrole) sql command.
- To drop a role, use [`drop role`](postgres/commands/droprole) sql command.
- To set the current role, use [`set role`](postgres/commands/setrole) sql command.

## References

- https://www.postgresql.org/docs/current/user-manag.html
