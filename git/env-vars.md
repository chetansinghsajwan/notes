# Git Environment Variables

**Todo**:: List the important environment variables here.

- **`GIT_EXEC_PATH`** determines where Git looks for its sub-programs (like `git-commit`, `git-diff`, and others). You can check the current setting by running `git --exec-path`.

- **`HOME`** isn’t usually considered customizable (too many other things depend on it), but it’s where Git looks for the global configuration file.

- **`PREFIX`** is similar, but for the system-wide configuration. Git looks for this file at `$PREFIX/etc/gitconfig`.

- **`GIT_CONFIG_NOSYSTEM`**, if set, disables the use of the system-wide configuration file. This is useful if your system config is interfering with your commands, but you don’t have access to change or remove it.

- **`GIT_PAGER`** controls the program used to display multi-page output on the command line. If this is unset, `PAGER` will be used as a fallback.

- **`GIT_EDITOR`** is the editor Git will launch when the user needs to edit some text (a commit message, for example). If unset, `EDITOR` will be used.

## References

- https://git-scm.com/book/ms/v2/Git-Internals-Environment-Variables
