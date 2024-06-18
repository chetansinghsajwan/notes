# `block` command

Evaluate a group of commands with a dedicated variable and/or policy scope.

## Syntax

```cmake
block([SCOPE_FOR [POLICIES] [VARIABLES] ] [PROPAGATE <var-name>...])
  <commands>
endblock()
```

All commands between `block()` and the matching [`endblock()`](https://cmake.org/cmake/help/latest/command/endblock.html#command:endblock) are recorded without being invoked. Once the [`endblock()`](https://cmake.org/cmake/help/latest/command/endblock.html#command:endblock) is evaluated, the recorded list of commands is invoked inside the requested scopes, then the scopes created by the `block()` command are removed.

## Parameters

> ##### `SCOPE_FOR`
> 
> Specify which scopes must be created.

> ##### `POLICIES`
> 
> Create a new policy scope.

> ##### `VARIABLES`
> 
> Create a new variable scope.
> 
> If `SCOPE_FOR` is not specified, this is equivalent to.
> 
> ```cmake
> block(SCOPE_FOR VARIABLES POLICIES)
> ```

> ##### `PROPOGATE`
> 
> When a variable scope is created by the [`block()`](https://cmake.org/cmake/help/latest/command/block.html#command:block) command, this option sets or unsets the specified variables in the parent scope.
> 
> > [!example]
> > 
> > ```cmake
> > set(var1 "INIT1")
> > set(var2 "INIT2")
> > 
> > block(PROPAGATE var1 var2)
> > 	set(var1 "VALUE1")
> > 	unset(var2)
> > endblock()
> > 
> > # Now var1 holds VALUE1, and var2 is unset
> > ```
> 
> This option is only allowed when a variable scope is created. An error will be raised in the other cases.
> 
> When the `block()` is inside a `foreach()` or `while()` command, the `break()` and `continue()` commands can be used inside the block.

## References

- https://cmake.org/cmake/help/latest/command/block.html