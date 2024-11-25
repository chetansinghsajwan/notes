# User Commands File

The profile loaded for the current user in interactive shells for running commands.

The file set by [`--rcfile`](bash/shell/options/rcfile) or [`--init-file`](bash/shell/options/init-file) options is considered user commands fille. By default this file is set to `~/.bashrc`.

This file is loaded only if the shell is:
- [interactive](bash/shell/modes/interactive.md), and
- not [login shell](bash/shell/modes/login.md), and
- not in [posix mode](bash/shell/modes/posix.md), and
- not run as [`sh`](#^sh), and
- not run with [`--norc`](bash/shell/options/norc) option

## References

- https://www.gnu.org/software/bash/manual/bash.html#Bash-Startups
- https://gist.github.com/ChrisTollefson/6294f711922c88ce3aef2e420452eba1
