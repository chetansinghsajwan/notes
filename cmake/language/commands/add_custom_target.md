# `add_custom_target` command

Adds a target with the given name that executes the given commands. The target has no output file and is _always considered out of date_ even if the commands try to create a file with the name of the target. Use the [`add_custom_command`](add_custom_command.md) command to generate a file with dependencies. By default nothing depends on the custom target. Use the [`add_dependencies`](add_dependencies.md) command to add dependencies to or from other targets.

## Syntax

```cmake
add_custom_target(Name [ALL] [command1 [args1...]]
                  [COMMAND command2 [args2...] ...]
                  [DEPENDS depend depend depend ... ]
                  [BYPRODUCTS [files...]]
                  [WORKING_DIRECTORY dir]
                  [COMMENT comment]
                  [JOB_POOL job_pool]
                  [JOB_SERVER_AWARE <bool>]
                  [VERBATIM] [USES_TERMINAL]
                  [COMMAND_EXPAND_LISTS]
                  [SOURCES src1 [src2...]])
```

## Parameters

> ###### `ALL` (optional)
> 
> Adds this target to the [all target](unknown/cmake-language/all-target).

> ###### `COMMENT` (optional)
> 
> Display the given message before the commands are executed at build time.

> ###### `COMMAND_EXPAND_LISTS` (optional)
> 
> Lists in `COMMAND` arguments will be expanded.

> ###### `SOURCES` (optional)
> 
> Specify additional source files to be included in the custom target. 
> 
> Specified source files will be added to IDE project files for convenience in editing even if they have no build rules.

> ###### `VERBATIM` (optional) (recommended)
> 
> All arguments to the commands will be escaped properly for the build tool so that the invoked command receives each argument unchanged. 
> 
> Use of `VERBATIM` is recommended as it enables correct behavior. When `VERBATIM` is not given the behavior is platform specific because there is no protection of tool-specific special characters.

> ###### `USES_TERMINAL` (optional)
> 
> The command will be given direct access to the terminal if possible.

> ###### `WORKING_DIRECTORY` (optional)
> 
> Execute the command with the given current working directory

## References

- https://cmake.org/cmake/help/latest/command/add_custom_target.html