# Branch

A branch represents a line of work. In implementation, a branch is just a [reference](ref.md) to the latest commit in that line of work.

When a branch is checked out, the [head](git/head) points to the name of that branch (not the commit branch points to). This state moves the branch reference to point to the next commit when a new commit is made.

## References

- https://git-scm.com/book/en/v2/Git-Internals-Git-References
