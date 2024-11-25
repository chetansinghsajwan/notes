# User Profile File

This file is used to define user specific configuration.

Each of the following files is considered user profile file, and only the file found first is loaded and rest are ignored. The search happens in the following order:

- `~/.bash_profile`, if not running as [`sh`](#^sh)
- `~/.bash_login`, if not running as [`sh`](#^sh)
- `~/.profile`

These files are loaded only if the shell is:
- [login shell](bash/shell/modes/login.md), and
- not in [posix mode](bash/shell/modes/posix.md), and
- not run with [`--noprofile`](bash/shell/options/noprofile) option

This file is often used to source [user-commands-file](#^user-commands-file), as both are used to store the same configuration.

## References

- https://www.gnu.org/software/bash/manual/bash.html#Bash-Startups
- https://gist.github.com/ChrisTollefson/6294f711922c88ce3aef2e420452eba1
