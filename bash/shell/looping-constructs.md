# Looping Constructs

Bash supports the following looping constructs.

Note that wherever a ‘;’ appears in the description of a command’s syntax, it may be replaced with one or more newlines.

###### `until`

**Syntax:** `until` test-commands `; do ` consequent-commands `; done`

Execute consequent-commands as long as test-commands has an exit status which is not zero. The return status is the exit status of the last command executed in consequent-commands, or zero if none was executed.

###### `while`

**Syntax:** `while` test-commands `; do` consequent-commands `; done`

Execute consequent-commands as long as test-commands has an exit status of zero. The return status is the exit status of the last command executed in consequent-commands, or zero if none was executed.

###### `for`

**Syntax:** `for` name [ [in [words …] ] ; ] `do` commands `; done`

Expand words (see [Shell Expansions](https://www.gnu.org/software/bash/manual/bash.html#Shell-Expansions)), and execute commands once for each member in the resultant list, with name bound to the current member. If ‘in words’ is not present, the `for` command executes the commands once for each positional parameter that is set, as if ‘in "$@"’ had been specified (see [Special Parameters](https://www.gnu.org/software/bash/manual/bash.html#Special-Parameters)).

The return status is the exit status of the last command that executes. If there are no items in the expansion of words, no commands are executed, and the return status is zero.

An alternate form of the `for` command is also supported:

**Syntax:**

`for ((` expr1 `;` expr2 `;` expr3 `)) ; do` commands `; done`

First, the arithmetic expression expr1 is evaluated according to the rules described below (see [Shell Arithmetic](https://www.gnu.org/software/bash/manual/bash.html#Shell-Arithmetic)). The arithmetic expression expr2 is then evaluated repeatedly until it evaluates to zero. Each time expr2 evaluates to a non-zero value, commands are executed and the arithmetic expression expr3 is evaluated. If any expression is omitted, it behaves as if it evaluates to 1. The return value is the exit status of the last command in commands that is executed, or false if any of the expressions is invalid.

The `break` and `continue` builtins (see [Bourne Shell Builtins](https://www.gnu.org/software/bash/manual/bash.html#Bourne-Shell-Builtins)) may be used to control loop execution.