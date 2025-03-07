# Git `switch` command

This command is used to switch branches.

## Synopsis

```shell
git switch [<options>] [--no-guess] <branch>
git switch [<options>] --detach [<start-point>]
git switch [<options>] (-c|-C) <new-branch> [<start-point>]
git switch [<options>] --orphan <new-branch>
```

## Description

Switching branches does not require a clean index and working tree (i.e. no differences compared to `HEAD`). The operation is aborted however if the operation leads to loss of local changes, unless told otherwise with `--`.

---
```shell
git switch [<options>] (-c|-C) <new-branch> [<start-point>]
```

This form is used to create a new branch wit name `<new-branch>`. 

## References

- https://git-scm.com/docs/git-switch
