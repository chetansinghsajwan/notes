# Git Credentials

Without any credential helpers defined, Git will try the following strategies to ask the user for usernames and passwords:

1. If the `GIT_ASKPASS` environment variable is set, the program specified by the variable is invoked. A suitable prompt is provided to the program on the command line, and the user’s input is read from its standard output.
    
2. Otherwise, if the `core.askPass` configuration variable is set, its value is used as above.
    
3. Otherwise, if the `SSH_ASKPASS` environment variable is set, its value is used as above.
    
4. Otherwise, the user is prompted on the terminal.

## References

- https://git-scm.com/docs/gitcredentials
