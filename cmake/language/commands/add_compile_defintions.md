# `add_compile_definitions` command

Adds preprocessor definitions to the compiler command line.

The preprocessor definitions are added to the [compile_defintions](cmake/language/properties/directory/compile_defintions.md) directory property for the current `CMakeLists` file.

They are also added to the [compile_defintions](cmake/language/properties/target/compile_defintions.md) target property for each target in the current `CMakeLists` file.

## Syntax

```cmake
add_compile_definitions(<definition> ...)
```

## Parameters

> ##### `<definition>...`
> 
> Definitions are specified using the syntax `VAR` or `VAR=value`. 
> 
> Function-style definitions are not supported.
> **
> Any leading `-D` on an item will be removed.

## References

- https://cmake.org/cmake/help/latest/command/add_compile_definitions.html