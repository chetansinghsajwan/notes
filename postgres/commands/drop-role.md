# `DROP ROLE` Command

Not bef

A role cannot be removed if it is still referenced in any database of the cluster; an error will be raised if so. Before dropping the role, you must drop all the objects it owns (or reassign their ownership) and revoke any privileges the role has been granted on other objects. The [`REASSIGN OWNED`](https://www.postgresql.org/docs/current/sql-reassign-owned.html "REASSIGN OWNED") and [`DROP OWNED`](https://www.postgresql.org/docs/current/sql-drop-owned.html "DROP OWNED") commands can be useful for this purpose; see [Section 21.4](https://www.postgresql.org/docs/current/role-removal.html "21.4. Dropping Roles") for more discussion.

## References

- https://www.postgresql.org/docs/current/sql-droprole.html
