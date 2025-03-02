# Git Credentials

Git supports external tools to store and access credentials, these tools are called credential helpers.

Without any credential helpers defined, Git will try the following strategies to ask the user for usernames and passwords:

1. If the `GIT_ASKPASS` environment variable is set, the program specified by the variable is invoked. A suitable prompt is provided to the program on the command line, and the user’s input is read from its standard output.
2. Otherwise, if the `core.askPass` configuration variable is set, its value is used as above.
3. Otherwise, if the `SSH_ASKPASS` environment variable is set, its value is used as above.
4. Otherwise, the user is prompted on the terminal.

The credential helper to use can be defined in the configuration using:

```
credential.helper <helper>
```

By default, git comes with two credential helpers:

- cache
	
	Cache credentials in memory for a short period of time. See [git-credential-cache](git/credential-cache) for details.

- store
	
	Store credentials on disk in plain text. See [git-credential-store](git/credential-store) for details.

## References

- https://git-scm.com/docs/gitcredentials
