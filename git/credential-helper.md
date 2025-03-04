# Git Credential Helper

Git supports external tools to store and access credentials, these tools are called credential helpers.

The credential helper to use is defined using the following configuration option:

```
credential.helper <helper>
```

This option can be specified multiple times, in which case each entry will be tried in sequence until one helper returns the credentials.

The `<helper>` string is transformed by Git into a command to be executed using these rules:

1. If the helper string begins with "!", it is considered a shell snippet, and everything  after the "!" becomes the command.
2. Otherwise, if the helper string begins with an absolute path, the verbatim helper string becomes the command.
3. Otherwise, the string "git credential-" is prepended to the helper string, and the result becomes the command.

Here are some example specifications:

```shell
# run "git credential-foo"
[credential]
	helper = foo

# same as above, but pass an argument to the helper
[credential]
	helper = "foo --bar=baz"

# the arguments are parsed by the shell, so use shell
# quoting if necessary
[credential]
	helper = "foo --bar='whitespace arg'"

# store helper (discouraged) with custom location for the db file;
# use `--file ~/.git-secret.txt`, rather than `--file=~/.git-secret.txt`,
# to allow the shell to expand tilde to the home directory.
[credential]
	helper = "store --file ~/.git-secret.txt"

# you can also use an absolute path, which will not use the git wrapper
[credential]
	helper = "/path/to/my/helper --with-arguments"

# or you can specify your own shell snippet
[credential "https://example.com"]
	username = your_user
	helper = "!f() { test \"$1\" = get && echo \"password=$(cat $HOME/.secret)\"; }; f"
```

When a helper is executed, it will have one "operation" argument appended to its command line, which is one of:

- `get`: Return a matching credential, if any exists.
- `store`: Store the credential, if applicable to the helper.
- `erase`: Remove matching credentials, if any, from the helper’s storage.

The credentials are read and write using helper's `stdin` and `stdout` streams. See [input/output format](credential-helper-iofmt.md) of credential helper to learn more.

---

By default, git comes with two credential helpers:

- `cache`: Cache credentials in memory for a short period of time. See [git-credential-cache](git/credential-cache) for details.
- `store`: Store credentials on disk in plain text. See [git-credential-store](git/credential-store) for details.

You can use `git help -a | grep credential-` to list all available credential helpers.

The community maintains a comprehensive list of Git credential helpers at https://git-scm.com/doc/credential-helpers.

## References

- https://git-scm.com/docs/gitcredentials
