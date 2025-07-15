# `readline` Library

It is a library that provides in-line editing and history capabilities for interactive programs with a command-line interface, such as Bash.

This libray is a part of the gnu project.

It supports two editing modes - emacs and vi.

It reads inputrc from the following locations for configuration:
1. Path specified by `INPUTRC` env variable.
2. If the variable is not set, then reads `~/.inputrc` file.
3. If the above file is not found or read, then reads `/etc/inputrc` file.

See https://tiswww.case.edu/php/chet/readline/readline.html for configuration options.

## References

- https://www.wikiwand.com/en/articles/GNU_Readline
- https://tiswww.case.edu/php/chet/readline/rltop.html
- https://tiswww.case.edu/php/chet/readline/readline.html
