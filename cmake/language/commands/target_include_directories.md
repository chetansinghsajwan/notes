# `target_include_directories` command

Specifies include directories to use when compiling a given target.

## Syntax

```cmake
target_include_directories(<target> [SYSTEM] [AFTER|BEFORE]
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])
```

## Parameters

> ##### `<target>` #required 
> 
> The target to set include directories for. The named `<target>` must have been created by a command such as [[add_executable]] or [[add_library]] and must not be an [alias-target]().

> ##### `[SYSTEM]` #optional 
> 
> If `SYSTEM` is specified, the compiler will be told the directories are meant as system include directories on some platforms. This may have effects such as suppressing warnings or skipping the contained headers in dependency calculations (see compiler documentation). 
> 
> Additionally, system include directories are searched after normal include directories regardless of the order specified.
> 
> If `SYSTEM` is used together with `PUBLIC` or `INTERFACE`, the [INTERFACE_SYSTEM_INCLUDE_DIRECTORIES]() target property will be populated with the specified directories.

> ##### `[AFTER|BEFORE]` #optional 
> 
> `AFTER` or `BEFORE` is used to append or prepend the include directories respectively.
> 
> By default `AFTER` is selected.

> ##### `<INTERFACE|PUBLIC|PRIVATE> [items1...]` #required 
> 
> `PRIVATE` and `PUBLIC` items will populate the [`INCLUDE_DIRECTORIES`](programming/languages/cmake/properties/target/include_directories) property of `<target>`.
> 
> `PUBLIC` and `INTERFACE` items will populate the [`INTERFACE_INCLUDE_DIRECTORIES`](programming/languages/cmake/properties/target/interface_include_directories) property of `<target>`.

## References

- https://cmake.org/cmake/help/latest/command/target_include_directories.html