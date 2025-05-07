# Roles

PostgreSQL manages database access permissions using the concept of _roles_. A role can be thought of as either a database user, or a group of database users, depending on how the role is set up. Roles can own database objects (for example, tables and functions) and can assign privileges on those objects to other roles to control who has access to which objects. Furthermore, it is possible to grant _membership_ in a role to another role, thus allowing the member role to use privileges assigned to another role.

The concept of roles subsumes the concepts of “users” and “groups”. In PostgreSQL versions before 8.1, users and groups were distinct kinds of entities, but now there are only roles. Any role can act as a user, a group, or both.

Every connection to the database server is made using the name of some particular role.

- To create a role, use [`create role`](create-role.md) sql command.
- To alter a role, use [`alter role`](postgres/commands/alterrole) sql command.
- To drop a role, use [`drop role`](postgres/commands/droprole) sql command.
- To set the current role, use [`set role`](postgres/commands/setrole) sql command.

To list existing roles, examine the `pg_roles` system catalog, for example:

```sql
SELECT rolname FROM pg_roles;
```

For convenience, the programs [createuser](https://www.postgresql.org/docs/current/app-createuser.html "createuser") and [dropuser](https://www.postgresql.org/docs/current/app-dropuser.html "dropuser") are provided as wrappers around these SQL commands that can be called from the shell command line:

In order to bootstrap the database system, a freshly initialized system always contains one predefined login-capable role. This role is always a “superuser”, and it will have the same name as the operating system user that initialized the database cluster with `initdb` unless a different name is specified. This role is often named `postgres`. In order to create more roles you first have to connect as this initial role.

## References

- https://www.postgresql.org/docs/current/user-manag.html
