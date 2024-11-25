# Reference

A reference is a simple name that points to a commit.

This name makes it easy for us humans to remember a commit by a name instead of its hash.

These references are stored inside `.git/refs/` directory as files. Each file stores only the hash of the commit.

Refs are categorized and stored into following directories:

- `heads/`
- `tags/`
- `remotes/`

## `heads`

These are the refs that are the heads of your local branches.

## `tags`

These are the refs that are your tags.

See [tag](tag.md) for more info.

## `remotes`

These are the refs for remote history. Git manages them internally.

## References

- https://git-scm.com/book/en/v2/Git-Internals-Git-References
