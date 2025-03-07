# Git Config

Git divides the configuration in three levels:

1. System Level
2. Global or User Level
3. Local Level

System Level is applied throughout the whole system, for each user. The configuration file for this level is stored at `/etc/gitconfig` file.

Global Level is specific to each user. The configuration file for this level is stored at `~/.gitconfig` or `~/.config/git/config`.

Local level is specific to each repository. The configuration file for this level is stored at `.git/config`.

You can use `git config` to read and write configuration for each level using the flags `--system`, `--global` and `--local`. `--local` is the default flag if no flag is specified.

The lower level overrides values in the higher level, which means local has the highest priority, and system has the least.

## References

- https://git-scm.com/book/ms/v2/Customizing-Git-Git-Configuration
- https://git-scm.com/docs/git-config
