# Bash Env File

If the [`BASH_ENV`](bash/shell/variables/bash_env) env variable is set, runs the file set in the value.

This file is only loaded if the shell is:
- not [interactive](bash/shell/modes/interactive.md), and
- not in [posix mode](bash/shell/modes/posix.md), and
- not run as [`sh`](#^sh)

## References

- https://www.gnu.org/software/bash/manual/bash.html#Bash-Startups
- https://gist.github.com/ChrisTollefson/6294f711922c88ce3aef2e420452eba1
