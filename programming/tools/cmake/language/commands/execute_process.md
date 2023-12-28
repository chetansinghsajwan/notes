# `execute_process` command

## Syntax

```cmake
execute_process(COMMAND <cmd1> [<arguments>]
                [COMMAND <cmd2> [<arguments>]]...
                [WORKING_DIRECTORY <directory>]
                [TIMEOUT <seconds>]
                [RESULT_VARIABLE <variable>]
                [RESULTS_VARIABLE <variable>]
                [OUTPUT_VARIABLE <variable>]
                [ERROR_VARIABLE <variable>]
                [INPUT_FILE <file>]
                [OUTPUT_FILE <file>]
                [ERROR_FILE <file>]
                [OUTPUT_QUIET]
                [ERROR_QUIET]
                [COMMAND_ECHO <where>]
                [OUTPUT_STRIP_TRAILING_WHITESPACE]
                [ERROR_STRIP_TRAILING_WHITESPACE]
                [ENCODING <name>]
                [ECHO_OUTPUT_VARIABLE]
                [ECHO_ERROR_VARIABLE]
                [COMMAND_ERROR_IS_FATAL <ANY|LAST>])
```

## Parameters

> ##### `WORKING_DIRECTORY` (optional)
> 
> The named directory will be set as the current working directory of the child processes.

> ##### `TIMEOUT` (optional)
> 
> After the specified number of seconds (fractions allowed), all unfinished child processes will be terminated, and the `RESULT_VARIABLE` will be set to a string mentioning the "timeout".

> ##### `RESULT_VARIABLE <variable>` (optional)
> 
> The variable will be set to contain the result of last child process. This will be an integer return code from the last child or a string describing an error condition.`

> ##### `RESULTS_VARIABLE <variable>` (optional)
> 
> The variable will be set to contain the result of all processes as a [semicolon-separated list](cmake-language/lists), in order of the given `COMMAND` arguments.
> 
> Each entry will be an integer return code from the corresponding child or a string describing an error condition.

> ##### `INPUT_FILE <file>` (optional)
> 
> `<file>` is attached to the standard input pipe of the first `COMMAND` process.

> ##### `OUTPUT_FILE <file>` (optional)
> 
> `<file>` is attached to the standard output pipe of the last `COMMAND` process.

> ##### `ERROR_FILE <file>` (optional)
> 
> `<file>` is attached to the standard error pipe of all `COMMAND` processes.
> 
> If the same `<file>` is named for both `OUTPUT_FILE` and `ERROR_FILE` then it will be used for both standard output and standard error pipes for all commands.

> ##### `COMMAND_ERROR_IS_FATAL <ANY|LAST>` (optional)
> 
> Determines the behavior when an error is encountered:
> 
> `ANY` If any of the commands in the list of commands fail, this command halts with an error.
> 
> `LAST` If the last command in the list of commands fails, this command halts with an error..

## References

- https://cmake.org/cmake/help/latest/command/execute_process.html