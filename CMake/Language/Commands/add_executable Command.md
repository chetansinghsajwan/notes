## Normal Executable
Adds an executable target called `<name>` to be built from the source files listed in the command invocation.
The target name has scope in the directory in which it is created and below.
### Syntax
```
`add_executable(<name> [WIN32] [MACOSX_BUNDLE] [EXCLUDE_FROM_ALL] [sources...])`
```
### Parameters

> `**<name>**` **(required)**
> 
> The name of the target and must be globally unique within a project.

> `**[WIN32]**`
> 
> The property `win32_executable` will be set on the target created.

> `**[MACOSX_BUNDLE]**`
> 
> The property `macosx_bundle` will be set on the target created.

> `**[EXCLUDE_FROM_ALL]**`
> 
> The property `exclude_from_all` will be set on the target created.

> `**[sources...]**`
> 
> List of source files.
> 
> The source files can be omitted if they are added later using `target_sources()`.
## Imported Target
Creates an imported target of name `<name>` for `<target>`.
### Syntax
```
`add_executable(<name> IMPORTED [GLOBAL])
```
### Parameters

> `**<name>**`
> 
> The `<name>` does not appear in the generated build-system as a make target.

> `**[GLOBAL]**`
> 
> The global option extends visibility to global level.
## Alias Target
Creates an alias target of name `<name>` for `<target>`.
### Syntax
```
add_executable(<name> ALIAS <target>)
```
### Parameters

> `**<name>**`
> 
> The `<name>` does not appear in the generated build-system as a make target.

> `**<target>**`
> 
> The `<target>` itself cannot be an `ALIAS`.
## References
- https://cmake.org/cmake/help/latest/command/add_executable.html