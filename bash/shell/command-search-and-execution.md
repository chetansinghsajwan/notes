# Command Search and Execution

After a command has been split into words, if it results in a simple command and an optional list of arguments, the following actions are taken.

1. If the command name contains no slashes, the shell attempts to locate it. If there exists a shell function by that name, that function is invoked as described in [Shell Functions](https://www.gnu.org/software/bash/manual/bash.html#Shell-Functions).

2. If the name does not match a function, the shell searches for it in the list of shell builtins. If a match is found, that builtin is invoked.

3. If the name is neither a shell function nor a builtin, and contains no slashes, Bash searches each element of `$PATH` for a directory containing an executable file by that name. Bash uses a hash table to remember the full pathnames of executable files to avoid multiple `PATH` searches (see the description of `hash` in [Bourne Shell Builtins](https://www.gnu.org/software/bash/manual/bash.html#Bourne-Shell-Builtins)). A full search of the directories in `$PATH` is performed only if the command is not found in the hash table. If the search is unsuccessful, the shell searches for a defined shell function named `command_not_found_handle`. If that function exists, it is invoked in a separate execution environment with the original command and the original command’s arguments as its arguments, and the function’s exit status becomes the exit status of that subshell. If that function is not defined, the shell prints an error message and returns an exit status of 127.

4. If the search is successful, or if the command name contains one or more slashes, the shell executes the named program in a separate execution environment. Argument 0 is set to the name given, and the remaining arguments to the command are set to the arguments supplied, if any.

5. If this execution fails because the file is not in executable format, and the file is not a directory, it is assumed to be a _shell script_ and the shell executes it as described in [Shell Scripts](https://www.gnu.org/software/bash/manual/bash.html#Shell-Scripts).

6. If the command was not begun asynchronously, the shell waits for the command to complete and collects its exit status.

## References

- https://www.gnu.org/software/bash/manual/bash.html#Command-Search-and-Execution