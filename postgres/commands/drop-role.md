# `DROP ROLE` Command

```
DROP ROLE [ IF EXISTS ] <name> [, ...]
```

Drops the specified role.

Before a role can be dropped, following conditions must be met:

- The role must not own any database objects.
- The role must not have any privileges.

The [`REASSIGN OWNED`](postgres/commands/reassign-owned) and [`DROP OWNED`](postgres/commands/drop-role) commands can be useful for this purpose.

However, it is not necessary to remove role memberships involving the role; `DROP ROLE` automatically revokes any memberships of the target role in other roles, and of other roles in the target role. The other roles are not dropped nor otherwise affected.

## References

- https://www.postgresql.org/docs/current/sql-droprole.html
- https://www.postgresql.org/docs/current/role-removal.html
