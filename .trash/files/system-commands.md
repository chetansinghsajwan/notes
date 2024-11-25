# System Commands File

`/etc/bash.bashrc` is the system commands file. It is loaded for all users in interactive shells for running commands.

This file is loaded only if the shell is:
- [interactive](bash/shell/modes/interactive.md), and
- not [login shell](bash/shell/modes/login.md), and
- not in [posix mode](bash/shell/modes/posix.md), _and_
- not run as [`sh`](#^sh), and
- not run with [`--norc`](bash/shell/options/norc)

> [!note]
> Loading this file is not the default behvaiour of `bash`. This feature was introduced by the debian distribution in their releases of `bash` and later was adopted by other distributions as well. So this feature may not be available in all `bash` shells.

## References

- https://www.gnu.org/software/bash/manual/bash.html#Bash-Startups
- https://gist.github.com/ChrisTollefson/6294f711922c88ce3aef2e420452eba1
- https://askubuntu.com/questions/815066/whats-the-difference-between-bashrc-and-etc-bash-bashrc
