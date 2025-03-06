# Git Head

Head is just a reference to a commit. Git uses this reference to determine the current state of the repository.

For example, Git uses head to compare Head Tree with the working tree to determine what are the changes made in the working tree, that is, how is working tree different from the commit head points to.

The reference file for head is stored at `.git/HEAD`.

The head can refer to either a branch or the commit. If it refers to a branch, any commit moves the branch forward.

If it refers to a commit, the head is said to be in a detached head state. In this case, any commit just moves head to the new commit.

## References

- https://git-scm.com/book/ms/v2/Git-Internals-Git-References
