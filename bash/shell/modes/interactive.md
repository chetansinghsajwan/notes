# Interactive Mode

It is a state of the shell that is intended to interact with the user. It generally reads from and writes to a user’s terminal.

##### Check if this shell is interactive

Test the value of the ‘-’ special parameter. If it contains `i` then the shell is interactive. For example:

```bash
case "$-" in
*i*)    echo This shell is interactive ;;
*)      echo This shell is not interactive ;;
esac
```

Alternatively, examine the variable `PS1`; it is set in interactive shells. Thus:

```bash
if [ -z "$PS1" ]; then
    echo This shell is not interactive
else
    echo This shell is interactive
fi
```

##### Behaviour

Some important behvaiours are listed below. For a full list of behaviours, see [all behaviours](https://www.gnu.org/software/bash/manual/bash.html#Interactive-Shell-Behavior).

- Startup files are read and executed as described in [startup](bash/shell/startup.md).

- Bash expands and displays `PS1` before reading the first line of a command, and expands and displays `PS2` before reading the second and subsequent lines of a multi-line command. Bash expands and displays `PS0` after it reads a command but before executing it. See [Controlling the Prompt](https://www.gnu.org/software/bash/manual/bash.html#Controlling-the-Prompt), for a complete list of prompt string escape sequences.

- Command history (see [Bash History Facilities](https://www.gnu.org/software/bash/manual/bash.html#Bash-History-Facilities)) and history expansion (see [History Expansion](https://www.gnu.org/software/bash/manual/bash.html#History-Interaction)) are enabled by default. Bash will save the command history to the file named by `$HISTFILE` when a shell with history enabled exits.

- Alias expansion (see [Aliases](https://www.gnu.org/software/bash/manual/bash.html#Aliases)) is performed by default.

## References

- https://www.gnu.org/software/bash/manual/bash.html#Interactive-Shells