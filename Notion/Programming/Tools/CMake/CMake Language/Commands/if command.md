---
tags:
  - review
---

### Condition Syntax

Compound conditions are evaluated in the following order of precedence.

### Basic Expressions

##### `if (<constant>)`

True if the constant is `1`, `ON`, `YES`, `TRUE`, `Y`, or a non-zero number (including floating point numbers).

False if the constant is `0`, `OFF`, `NO`, `FALSE`, `N`, `IGNORE`, `NOTFOUND`, the empty string, or ends in the suffix `-NOTFOUND`.

Named boolean constants are case-insensitive.

If the argument is not one of these specific constants, it is treated as a variable or string.

##### `if (<variable>)`

`true` it the variable is defined and its value is not `false` constant, else `false`.

##### `if (<string>)`

`true` if the strings value is one of the `true` constants, else `false`.

### Logic Operators

##### `if (not <condition>)`

`true` if the condition is not true.

##### `if (<cond1> and <cond2>)`

`true` if both conditions are `true`.

##### `if (<cond1> or <cond2>)`

`true` if either condition is `true`.

### Numeric Comparisons
##### `if (<variable|string> <num-cmp-op> <variable|string>)`

where `<num-cmp-op>` is one of:
- `EQUAL`
- `LESS`
- `GREATER`
- `LESS_EQUAL`
- `GREATER_EQUAL`

`true` if the given string or variable's value parses as a real number and the relevant comparison is also `true`.

### String Comparisons

##### `if (<variable|string> <string-cmp-op> <variable|string>)`

where `<string-cmp-op>` is one of:
- `STREQUAL`
- `STRLESS`
- `STRGREATER`
- `STRLESS_EQUAL`
- `STRGREATER_EQUAL`

Performs relevant lexicographical comparison.

##### `if (<variable|string> MATCHES <regex>)`

`true` if the given string or variable's value matches the given regex.

> [!related]
> [regex](cmake-language/regex)

### Version Comparisons

##### `if (<variable|string> <version-cmp-op> <variable|string>)`

where `<version-cmp-op>` is one of:
- `VERSION_EQUAL`
- `VERSION_LESS`
- `VERSION_GREATER`
- `VERSION_LESS_EQUAL`
- `VERSION_GREATER_EQUAL`

Performs component-wise integer version number comparison.

Omitted components are treated as zero.

> [!note]
> Any non-integer version component or non-integer trailing part of a version component effectively truncates the string at that point.

### Path Comparisons

##### `if (<variable|string> PATH_EQUAL <variable|string>)`

Same as [`cmake_path(COMPARE <variable|string> EQUAL <variable|string>)`](cmake_path command/query)

### Existence Checks

##### `if (COMMAND <command-name>)`

`true` if the given name is a command, macro or function.

##### `if (POLICY <policy-id>)`

`true` if the given is a policy.

`<policy-id>` is of the from `CMP<NNNN>`.

##### `if (TARGET <target-name>)`

`true` if a target of name `<target-name>` exists.

##### `if (TEST <test-name>)`

`true` if a test of name `<test-name>` exists.

##### `if (DEFINED <name>)`

`true` if a variable, cache variable of environment variable of name `<name>` is defined.

##### `if (<variable|string> IN_LIST <variable>)`

`true` if the given element is contained in the named list `<variable>`.

### File Operations

##### `if (EXISTS <path-to-file-or-directory>)`

`true` if the named file or directory exists and is readable.

Resolves symbolic links, i.e. if the named file or directory is a symbolic link, returns true if the target of the symbolic link exists.

##### `if (<file1> IS_NEWER_THAN <file2>)`

`true` if `<file1>` is newer than or same old as `<file2>` or if one of the two files doesn't exist.

##### `if (IS_DIRECTORY <path>)`

`true` if the path exists and is a directory.

##### `if (IS_SYMLINK <path>)`

`true` if the given path is a symbolic link.

##### `if (IS_ABSOLUTE <path>)`

`true` if the given path is an absolute path and not empty.

#review
On windows, any `path` that begins with a drive letter and colon (e.g. `C:`), a forward slash or a backslash will evaluate to true. This means a path like `C:no\base\dir` will evaluate to true, even though the non-drive part of the path is relative.

On non windows, any `path` that begins with a tilde (`~`) evaluates to `true`.

# References

> [cmake-official-docs](https://cmake.org/cmake/help/latest/command/if.html)