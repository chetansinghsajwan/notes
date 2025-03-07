# Git Config

Git divides the configuration in three levels:

1. System Level
2. Global or User Level
3. Local Level
4. Worktree Level

System Level is applied throughout the whole system, for each user. The configuration file for this level is stored at `/etc/gitconfig` file.

Global Level is specific to each user. The configuration file for this level is stored at `$XDG_CONFIG_HOME/git/config` or `~/.gitconfig`, whichever is found first in that order.

Local level is specific to each repository. The configuration file for this level is stored at `$GIT_DIR/config`.

Worktree level is specific to all [worktrees](git/worktree) for each repository. The configuration file for this level is stored at `$GIT_DIR/config.worktree`.

You can use `git config` to read and write configuration for each level using the flags `--system`, `--global`, `--local` and `--worktree`. `--local` is the default flag if no flag is specified.

The lower level overrides values in the higher level, which means worktree has the highest priority, and system has the lowest.

You can use the following commands to manage configs:

- `git config get <name>`
- `git config set <name> <value>`
- `git config unset <name>`

See [git-config](https://git-scm.com/docs/git-config) for list of all configuration options.

## References

- https://git-scm.com/docs/git-config
