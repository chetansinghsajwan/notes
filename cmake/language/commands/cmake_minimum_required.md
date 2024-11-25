# `cmake_minimum_required` command

Sets the minimum required version of cmake for a project.

This command will set the value of the [cmake_minimum_required_version](cmake_minimum_required_version.md) variable to `<min>`.

This command implicitly invokes the [[cmake_policy]] command.

> [!note]
> 
> Call this command at the beginning of the top-level CMakeLists.txt file even before calling the [[cmake/language/commands/project]] command.
> 
> It is important to establish version and policy settings before invoking other commands whose behavior they may affect.  

> [!note]  
> 
> Do not call this command inside a function.

## Syntax

```cmake
cmake_minimum_required(VERSION <min>[...<policy_max>] [FATAL_ERROR])
```

## Parameters

> ##### `min` (required)
> 
> If the running version of CMake is lower than the `<min>` required version it will stop processing the project and report an error.

> ##### `policy_max` (optional)
> 
> Version passed to [[cmake_policy]] command as `<max>`.

## References

- https://cmake.org/cmake/help/latest/command/cmake_minimum_required.html