# `separate_arguments` command

Parse command-line arguments into a semicolon-separated list.

## Syntax

```cmake
separate_arguments(<variable> <mode> [PROGRAM [SEPARATE_ARGS]] <args>)
```

```cmake
separate_arguments(<var>)
```

Convert the value of `<var>` to a semi-colon separated list. All spaces are replaced with ';'. This helps with generating command lines.

## Parameters

##### `variable` (required)

Variable to store result in. This is a semi-colon separated list.

##### `mode` (required)

The exact parsing rules depend on the operating system.

They are specified by the `<mode>` argument which must be one of the following keywords:

- `UNIX_COMMAND`
  
  Arguments are separated by unquoted whitespace. Both single-quote and double-quote pairs are respected. A backslash escapes the next literal character (`\"` is `"`); there are no special escapes (`\n` is just `n`).

- `WINDOWS_COMMAND`
  
  A Windows command-line is parsed using the same syntax the runtime library uses to construct argv at startup. It separates arguments by whitespace that is not double-quoted. Backslashes are literal unless they precede double-quotes. See the MSDN article [Parsing C Command-Line Arguments](https://learn.microsoft.com/en-us/cpp/c-language/parsing-c-command-line-arguments) for details.

- `NATIVE_COMMAND`
  
  Proceeds as in `WINDOWS_COMMAND` mode if the host system is Windows. Otherwise proceeds as in `UNIX_COMMAND` mode.

##### `PROGRAM` (optional)

##### `SEPARATE_ARGS` (optional)

When this sub-option of `PROGRAM` option is specified, command-line arguments will be split as well and stored in `<variable>`.

> [!example]
> 
> ```cmake
> separate_arguments (out UNIX_COMMAND PROGRAM SEPARATE_ARGS "cc -c main.c")
> ```
> 
> The contents of `out` will be: `/path/to/cc;-c;main.c`

## References

- https://cmake.org/cmake/help/latest/command/separate_arguments.html#command:separate_arguments