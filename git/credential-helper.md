# Git Credentials

Git supports external tools to store and access credentials, these tools are called credential helpers.

The credential helper to use is defined using the following configuraiton option:

```
credential.helper <helper>
```

This option can be specified multiple times, in which case each entry will be tried in sequence until one helper returns some credentials.

Each helper is specified by a single string in the configuration variable `credential.helper` (and others, see [git-config[1]](https://git-scm.com/docs/git-config)). The string is transformed by Git into a command to be executed using these rules:

1. If the helper string begins with "!", it is considered a shell snippet, and everything after the "!" becomes the command.
    
2. Otherwise, if the helper string begins with an absolute path, the verbatim helper string becomes the command.
    
3. Otherwise, the string "git credential-" is prepended to the helper string, and the result becomes the command.
    

The resulting command then has an "operation" argument appended to it (see below for details), and the result is executed by the shell.
By default, git comes with two credential helpers:

- cache
	
	Cache credentials in memory for a short period of time. See [git-credential-cache](git/credential-cache) for details.

- store
	
	Store credentials on disk in plain text. See [git-credential-store](git/credential-store) for details.

You can use `git help -a | grep credential-` to list all available credential helpers.

The community maintains a comprehensive list of Git credential helpers at https://git-scm.com/doc/credential-helpers.

## References

- https://git-scm.com/docs/gitcredentials
