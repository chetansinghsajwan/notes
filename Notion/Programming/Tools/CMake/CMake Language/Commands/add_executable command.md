Add an executable to the project using the specified source files.

##### `add_executable(<name> [WIN32] [MACOSX_BUNDLE] [EXCLUDE_FROM_ALL] [sources...])`

Adds an executable target called `<name>` to be built from the source files listed in the command invocation.

##### Parameters

###### `<name>`

The name of the target and must be globally unique within a project.

###### `[WIN32]` #optional 

The property [`win32_executable`](cmake-language/target-properties/win32-executable) will be set on the target created.

###### `[MACOSX_BUNDLE]` #optional 

The property [`macosx_bundle`](cmake-language/target-properties/macosx-bundle) will be set on the target created.

###### `[EXCLUDE_FROM_ALL]` #optional 

The property [`exclude_from_all`](cmake-language/target-properties/exclude-from-all) will be set on the target created.

###### `[sources...]` #optional 

List of source files.

The source files can be omitted if they are added later using [`target_sources()`](cmake-commands/target-sources").

##### `add_executable(<name> IMPORTED [GLOBAL])`

Creates a [imported target](cmake-buildsystem/imported-targets) of name `<name>` for `<target>`.

##### `add_executable(<name> ALIAS <target>)`

Creates an [alias target](cmake-buildsystem/alias-target) of name `<name>` for `<target>`.