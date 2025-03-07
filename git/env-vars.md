# Git Environment Variables

**Todo**:: List the important environment variables here.

- **`GIT_EXEC_PATH`** determines where Git looks for its sub-programs (like `git-commit`, `git-diff`, and others). You can check the current setting by running `git --exec-path`.

- **`HOME`** isn’t usually considered customizable (too many other things depend on it), but it’s where Git looks for the global configuration file.

- **`PREFIX`** is similar, but for the system-wide configuration. Git looks for this file at `$PREFIX/etc/gitconfig`.

- **`GIT_CONFIG_NOSYSTEM`**, if set, disables the use of the system-wide configuration file. This is useful if your system config is interfering with your commands, but you don’t have access to change or remove it.

- **`GIT_PAGER`** controls the program used to display multi-page output on the command line. If this is unset, `PAGER` will be used as a fallback.

- **`GIT_EDITOR`** is the editor Git will launch when the user needs to edit some text (a commit message, for example). If unset, `EDITOR` will be used.

- **`GIT_INDEX_FILE`** is the path to the index file (non-bare repositories only).

- **`GIT_OBJECT_DIRECTORY`** can be used to specify the location of the directory that usually resides at `.git/objects`.

**`GIT_AUTHOR_NAME`** is the human-readable name in the “author” field.

**`GIT_AUTHOR_EMAIL`** is the email for the “author” field.

**`GIT_AUTHOR_DATE`** is the timestamp used for the “author” field.

**`GIT_COMMITTER_NAME`** sets the human name for the “committer” field.

**`GIT_COMMITTER_EMAIL`** is the email address for the “committer” field.

**`GIT_COMMITTER_DATE`** is used for the timestamp in the “committer” field.

## References

- https://git-scm.com/book/ms/v2/Git-Internals-Environment-Variables
