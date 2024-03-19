# Condition Expressions

#todo define this.

## Basic Expressions

##### `<constant>`
^constants

`true` if the constant is `1`, `ON`, `YES`, `TRUE`, `Y`, or a non-zero number (including floating point numbers).

`false` if the constant is `0`, `OFF`, `NO`, `FALSE`, `N`, `IGNORE`, `NOTFOUND`, the empty constantsstring, or ends in the suffix `-NOTFOUND`.

If the argument is not one of these specific constants, it is treated as a variable or string.

##### `<variable>`
^variable

`true` if the variable is defined and its value is not [false constant](#^constants).

##### `<string>`
^string

`true` if the strings value is one of the [true constants](#^constants).

## Logic Operators

##### `NOT <condition>`
^not

`true` if the condition is not true.

##### `<cond1> AND <cond2>`
^and

`true` if both conditions are `true`.

##### `<cond1> OR <cond2>`
^or

`true` if either condition is `true`.

##### `(condition) AND (condition OR (condition))`
^parenthesis

The conditions inside the parenthesis are evaluated first and then the remaining conditions are evaluated.

## Numeric Comparisons

##### `<variable|string> <num-cmp-op> <variable|string>`

where `<num-cmp-op>` is one of:
- `EQUAL`
- `LESS`
- `GREATER`
- `LESS_EQUAL`
- `GREATER_EQUAL`

`true` if the given string or variable's value parses as a real number and the relevant comparison is also `true`.

## String Comparisons

##### `<variable|string> <string-cmp-op> <variable|string>`

where `<string-cmp-op>` is one of:

- `STREQUAL`
- `STRLESS`
- `STRGREATER`
- `STRLESS_EQUAL`
- `STRGREATER_EQUAL`

Performs relevant lexicographical comparison.

If any argument is a defined variable, its value is used, else the literal itself is used.

##### `<variable|string> MATCHES <regex>`

`true` if the given string or variable's value matches the given [regex](cmake-langauge/regex).

## Version Comparisons

##### `<variable|string> <version-cmp-op> <variable|string>`

where `<version-cmp-op>` is one of:
- `VERSION_EQUAL`
- `VERSION_LESS`
- `VERSION_GREATER`
- `VERSION_LESS_EQUAL`
- `VERSION_GREATER_EQUAL`

Performs component-wise integer number comparison.

Omitted components are treated as zero.

If any argument is a defined variable, its value is used, else the literal itself is used.

> [!note]
> 
> Stops parsing the version components when first non-integer component is encountered.

## Path Comparisons

##### `<variable|string> PATH_EQUAL <variable|string>`

Same as [`cmake_path(COMPARE <variable|string> EQUAL <variable|string>)`](cmake_path.md#^query)

### Existence Checks

##### `COMMAND <command-name>`

`true` if the given name is a command, macro or function.

##### `POLICY <policy-id>`

`true` if the given id is a policy.

`<policy-id>` is of the from `CMP<NNNN>`.

##### `TARGET <target-name>`

`true` if a target of name `<target-name>` exists.

##### `TEST <test-name>`

`true` if a test of name `<test-name>` exists.

##### `DEFINED <name>`

`true` if a variable, cache variable of environment variable of name `<name>` is defined.

##### `<variable|string> IN_LIST <variable>`

`true` if the given element is contained in the named list `<variable>`.

## File Checks

##### `EXISTS <path-to-file-or-directory>`

`true` if the named file or directory exists and is readable.

Resolves symbolic links, i.e. if the named file or directory is a symbolic link, returns true if the target of the symbolic link exists.

##### `<file1> IS_NEWER_THAN <file2>`

`true` if `<file1>` is newer than or same old as `<file2>` or if one of the two files doesn't exist.

##### `IS_DIRECTORY <path>`

`true` if the path exists and is a directory.

##### `IS_SYMLINK <path>`

`true` if the given path is a symbolic link.

##### `IS_ABSOLUTE <path>`

`true` if the given path is an absolute path and not empty.

#review
On windows, any `path` that begins with a drive letter and colon (e.g. `C:`), a forward slash or a backslash will evaluate to true. This means a path like `C:no\base\dir` will evaluate to true, even though the non-drive part of the path is relative.

On non windows, any `path` that begins with a tilde (`~`) evaluates to `true`.

## Precedence

Compound conditions are evaluated in the following order of precedence.

1. [Parentheses](#parentheses).
    
2. Unary tests such as [EXISTS](#exists), [COMMAND](#command), and [DEFINED](#defined).
    
3. Binary tests such as [EQUAL](#equal), [LESS](#less), [LESS_EQUAL](#less-equal), [GREATER](#greater), [GREATER_EQUAL](#greater-equal), [STREQUAL](#strequal), [STRLESS](#strless-equal), [STRGREATER](#strgreater), [STRGREATER_EQUAL](#strgreater-equal), [VERSION_EQUAL](#version-equal), [VERSION_LESS](#version-less), [VERSION_LESS_EQUAL](#version-less-equal), [VERSION_GREATER](#version-greater), [VERSION_GREATER_EQUAL](#version-greater-equal), [PATH_EQUAL](#path-equal), and [MATCHES](#matches).
    
4. Unary logical operator [NOT](#not).
    
5. Binary logical operators [AND](#and) and [OR](#or), from left to right, without any short-circuit.

## References

- https://cmake.org/cmake/help/latest/command/if.html