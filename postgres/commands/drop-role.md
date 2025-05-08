# `DROP ROLE` Command

Before a role can be dropped, following conditions must be met:

- The role must not own any database objects.
- The role must not have any privileges.

The [`REASSIGN OWNED`](postgres/commands/reassign-owned) and [`DROP OWNED`](postgres/co) commands can be useful for this purpose; see [Section 21.4](https://www.postgresql.org/docs/current/role-removal.html "21.4. Dropping Roles") for more discussion.

## References

- https://www.postgresql.org/docs/current/sql-droprole.html
