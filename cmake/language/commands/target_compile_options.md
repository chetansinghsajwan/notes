# `target_compile_options` command

These options are used when compiling the given `<target>`.

## Syntax

```cmake
target_compile_options(<target> [BEFORE]
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])
```

## Parameters

> ###### `<target>`
> 
> The target to set options for.
> 
> The target cannot be an [alias-target](cmake-language/alias-target).

> ###### `[BEFORE]` (optional)
> 
> If `BEFORE` is specified, the content will be prepended to the property instead of being appended.

> ###### `<INTERFACE|PUBLIC|PRIVATE> <items...>` (required|optional)
> 
> `PRIVATE` and `PUBLIC` items will populate the [`COMPILE_OPTIONS`](cmake-language/variables/compile_options) property of `<target>`.
> 
> `PUBLIC` and `INTERFACE` items will populate the [`INTERFACE_COMPILE_OPTIONS`](cmake-language/variables/interface_compile_options) property of `<target>`.

## Option Deduplication

The final set of options used for a target is constructed by accumulating options from the current target and the usage requirements of its dependencies. The set of options is de-duplicated to avoid repetition.

## References

- https://cmake.org/cmake/help/latest/command/target_compile_options.html