# `target_compile-definitions` command

## Syntax

```cmake
target_compile_definitions(<target>
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])
```

## Parameters

> ##### `target` #required 
> 
> The target to set definitions for. The named `<target>` must have been created by a command such as [[add_executable]] or [[add_library]] and must not be an [alias-target]().

> ##### `<INTERFACE|PUBLIC|PRIVATE> <items>` #required 
> 
> List of compile definitions (macros) to set when compiling.
> 
> `INTERFACE`, `PUBLIC` and `PRIVATE` are used to specify the scope of the definitions.
> 
> `PRIVATE` and `PUBLIC` items will populate the [compile_defintions](cmake/language/properties/target/compile_defintions.md) property of the `<target>`.
> 
> `PUBLIC` and `INTERFACE` items will populate the [interface_compile_defintions](interface_compile_defintions.md) property of the `<target>`.
> 
> Repeated calls for the same `<target>` append items in the order called.
> 
> Any leading `-D` on an item will be removed. Empty items are ignored. 
> 
> For example, the following are all equivalent:
> 
> ```cmake
> target_compile_definitions(foo PUBLIC FOO)
> target_compile_definitions(foo PUBLIC -DFOO)  # -D removed
> target_compile_definitions(foo PUBLIC "" FOO) # "" ignored
> target_compile_definitions(foo PUBLIC -D FOO) # -D becomes "", then ignored
> ```
> 
> Definitions may optionally have values:
> 
> ```cmake
> target_compile_definitions(foo PUBLIC FOO=1)
> ```

## References

- https://cmake.org/cmake/help/latest/command/target_compile_definitions.html