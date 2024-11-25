# System Profile File

`/etc/profile` is the system profile file. It is loaded for all users when logging in and contains system wide configuration.

This file is loaded only if the shell is:
- [login shell](bash/shell/modes/login.md), and
- not in [posix mode](bash/shell/modes/posix.md), and
- not run with [`--noprofile`](bash/shell/options/noprofile) option

Generally this file sources files stored in `/etc/profile.d/*`. This is done to achieve a modular stucture.

## References

- https://www.gnu.org/software/bash/manual/bash.html#Bash-Startups
- https://gist.github.com/ChrisTollefson/6294f711922c88ce3aef2e420452eba1
