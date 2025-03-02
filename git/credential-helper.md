# Git Credential Helper

Git supports external tools to store and access credentials, these tools are called credential helpers.

The credential helper to use is defined using the following configuraiton option:

```
credential.helper <helper>
```

This option can be specified multiple times, in which case each entry will be tried in sequence until one helper returns some credentials.

The string is transformed by Git into a command to be executed using these rules:

1. If the helper string begins with "!", it is considered a shell snippet, and everything  after the "!" becomes the command.
2. Otherwise, if the helper string begins with an absolute path, the verbatim helper string becomes the command.
3. Otherwise, the string "git credential-" is prepended to the helper string, and the result becomes the command.

When a helper is executed, it will have one "operation" argument appended to its command line, which is one of:

- `get`: Return a matching credential, if any exists.
- `store`: Store the credential, if applicable to the helper.
- `erase`: Remove matching credentials, if any, from the helper’s storage.

By default, git comes with two credential helpers:

- `cache`: Cache credentials in memory for a short period of time. See [git-credential-cache](git/credential-cache) for details.
- `store`: Store credentials on disk in plain text. See [git-credential-store](git/credential-store) for details.

You can use `git help -a | grep credential-` to list all available credential helpers.

The community maintains a comprehensive list of Git credential helpers at https://git-scm.com/doc/credential-helpers.

## References

- https://git-scm.com/docs/gitcredentials
