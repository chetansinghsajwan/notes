# Reserved Words

Reserved words are words that have special meaning to the shell. They are used to begin and end the shell’s compound commands.

The following words are recognized as reserved when unquoted and the first word of a command: `if`, `then`, `elif`, `else`, `fi`, `time`, `for`, `in`, `untiil`, `while`, `do`, `done`, `case`, `esac`, `coproc`, `select`, `function`, `{`, `}`, `[[`, `]]`, `!`.

`in` is recognized as a reserved word if it is the third word of a `case` or `select` command. `in` and `do` are recognized as reserved words if they are the third word in a `for` command.

## References

- https://www.gnu.org/software/bash/manual/bash.html#Reserved-Words