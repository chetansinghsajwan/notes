# Env File

The profile loaded for [posix mode](bash/shell/modes/posix.md) shells. If the [`ENV`](bash/shell/variables/env) env variable is set, runs the file set in the value.

This file is loaded only if the shell is:
- [interactive](bash/shell/modes/interactive.md), and
- in [posix mode](bash/shell/modes/posix.md), or run as [`sh`](#^sh)

## References

- https://www.gnu.org/software/bash/manual/bash.html#Bash-Startups
- https://gist.github.com/ChrisTollefson/6294f711922c88ce3aef2e420452eba1
