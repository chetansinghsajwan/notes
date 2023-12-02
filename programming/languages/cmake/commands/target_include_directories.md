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
> By using `AFTER` or `BEFORE` explicitly, you can select between appending and prepending, independent of the default.

> ##### `<INTERFACE|PUBLIC|PRIVATE> [items1...]` #required 
> 
> The `INTERFACE`, `PUBLIC` and `PRIVATE` keywords are required to specify the [scope](https://cmake.org/cmake/help/latest/manual/cmake-buildsystem.7.html#target-usage-requirements) of the following arguments. `PRIVATE` and `PUBLIC` items will populate the [`INCLUDE_DIRECTORIES`](https://cmake.org/cmake/help/latest/prop_tgt/INCLUDE_DIRECTORIES.html#prop_tgt:INCLUDE_DIRECTORIES "INCLUDE_DIRECTORIES") property of `<target>`. `PUBLIC` and `INTERFACE` items will populate the [`INTERFACE_INCLUDE_DIRECTORIES`](https://cmake.org/cmake/help/latest/prop_tgt/INTERFACE_INCLUDE_DIRECTORIES.html#prop_tgt:INTERFACE_INCLUDE_DIRECTORIES "INTERFACE_INCLUDE_DIRECTORIES") property of `<target>`.

## References

> https://cmake.org/cmake/help/latest/command/target_include_directories.html