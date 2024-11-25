# `while` command

#todo add info about break and continue commands.

Evaluate a group of commands while a condition is true.

[[endwhile]] command is used to mark the end of the while loop.

The commands [[cmake/language/commands/break]] and [[continue]] provide means to escape from the normal control flow.

## Syntax

```cmake
while(<condition>)
  <commands>
endwhile()
```

## Parameters

> ##### `<condition>`
> 
> An expression of type [condition expressions](condition-expressions.md).

## References

- https://cmake.org/cmake/help/latest/command/while.html