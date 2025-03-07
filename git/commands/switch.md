# Git `switch` command

This command is used to switch branches.

## Synopsis

```text
git switch [<options>] [--no-guess] <branch>

# second form
git switch [<options>] --detach [<start-point>]

git switch [<options>] (-c|-C) <new-branch> [<start-point>]
git switch [<options>] --orphan <new-branch>
```

## Description

Switching branches does not require a clean index and working tree. The operation is aborted however if the operation leads to loss of local changes, unless told otherwise with `--discard-changes` or `--merge`.

---
```shell
git switch [<options>] (-c|-C) <new-branch> [<start-point>]
```

This form is used to create a new branch wit name `<new-branch>`. 

## References

- https://git-scm.com/docs/git-switch
