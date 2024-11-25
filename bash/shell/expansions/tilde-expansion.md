# Tilde Expansion

**todo**:: format this.

If a word begins with an unquoted tilde character (‘~’), all of the characters up to the first unquoted slash (or all characters, if there is no unquoted slash) are considered a **tilde-prefix**.

If none of the characters in the tilde-prefix are quoted, the characters in the tilde-prefix following the tilde are treated as a possible _login name_. If this login name is the null string, the tilde is replaced with the value of the `HOME` shell variable. If `HOME` is unset, the home directory of the user executing the shell is substituted instead. Otherwise, the tilde-prefix is replaced with the home directory associated with the specified login name.

If the characters following the tilde in the tilde-prefix consist of a number N, optionally prefixed by a ‘+’ or a ‘-’, the tilde-prefix is replaced with the corresponding element from the directory stack, as it would be displayed by the `dirs` builtin invoked with the characters following tilde in the tilde-prefix as an argument (see [The Directory Stack](https://www.gnu.org/software/bash/manual/bash.html#The-Directory-Stack)). If the tilde-prefix, sans the tilde, consists of a number without a leading ‘+’ or ‘-’, ‘+’ is assumed.

If the login name is invalid, or the tilde expansion fails, the word is left unchanged.

Each variable assignment is checked for unquoted tilde-prefixes immediately following a ‘:’ or the first ‘=’. In these cases, tilde expansion is also performed. Consequently, one may use filenames with tildes in assignments to `PATH`, `MAILPATH`, and `CDPATH`, and the shell assigns the expanded value.

The following table shows how Bash treats unquoted tilde-prefixes:

**`~`**: `$HOME`

**`~/foo`**: `$HOME/foo`

**`~fred/foo`**: The subdirectory `foo` of the home directory of the user `fred`

**`~+/foo`**: `$PWD/foo`

**`~-/foo`**: `${OLDPWD-'~-'}/foo`

**`~N`**: The string that would be displayed by `dirs +N`.

**`~+N`**: The string that would be displayed by `dirs +N`.

**`~-N`**: The string that would be displayed by `dirs -N`.

Bash also performs tilde expansion on words satisfying the conditions of variable assignments (see [Shell Parameters](https://www.gnu.org/software/bash/manual/bash.html#Shell-Parameters)) when they appear as arguments to simple commands. Bash does not do this, except for the declaration commands listed above, when in POSIX mode.

## References

- https://www.gnu.org/software/bash/manual/bash.html#Tilde-Expansion