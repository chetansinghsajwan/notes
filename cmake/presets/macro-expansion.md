# Macro Expansion

Macros are recognized in the form `$<macro-namespace>{<macro-name>}`.

Macros are evaluated in the context of the preset being used, even if the macro is in a field that was inherited from another preset.

For example, if the `Base` preset sets variable `PRESET_NAME` to `${presetName}`, and the `Derived` preset inherits from `Base`, `PRESET_NAME` will be set to `Derived`.

Recognized macros include:

###### `${sourceDir}`

Path to the project source directory (i.e. the same as [`CMAKE_SOURCE_DIR`](https://cmake.org/cmake/help/latest/variable/CMAKE_SOURCE_DIR.html#variable:CMAKE_SOURCE_DIR "CMAKE_SOURCE_DIR")).

###### `${sourceParentDir}`

Path to the project source directory's parent directory.

###### `${sourceDirName}`

The last filename component of `${sourceDir}`.

For example, if `${sourceDir}` is `/path/to/source`, this would be `source`.

###### `${presetName}`

Name specified in the preset's `name` field.

###### `${generator}`

Generator specified in the preset's `generator` field.

###### `${hostSystemName}`

- since: version 3

The name of the host operating system.

Contains the same value as [`CMAKE_HOST_SYSTEM_NAME`](https://cmake.org/cmake/help/latest/variable/CMAKE_HOST_SYSTEM_NAME.html#variable:CMAKE_HOST_SYSTEM_NAME).

###### `${fileDir}`

- since: version 4

Path to the directory containing the preset file which contains the macro.

###### `${dollar}`

A literal dollar sign (`$`).

###### `${pathListSep}`

- since: version 5

Native character for separating lists of paths, such as `:` or `;`.

For example, by setting `PATH` to `/path/to/ninja/bin${pathListSep}$env{PATH}`, `${pathListSep}` will expand to the underlying operating system's character used for concatenation in `PATH`.  

###### `$env{<variable-name>}`

Environment variable with name `<variable-name>`. The variable name may not be an empty string.

If the variable is defined in the `environment` field, that value is used instead of the value from the parent environment.

If the environment variable is not defined, this evaluates as an empty string.

> [!note]
> While Windows environment variable names are case-insensitive, variable names within a preset are still case-sensitive. This may lead to unexpected results when using inconsistent casing. For best results, keep the casing of environment variable names consistent.

###### `$penv{<variable-name>}`

Similar to `$env{<variable-name>}`, except that the value only comes from the parent environment, and never from the `environment` field.

For example, setting `PATH` to `/path/to/ninja/bin:$penv{PATH}` will prepend `/path/to/ninja/bin` to the `PATH` environment variable. This is needed because `$env{<variable-name>}` does not allow circular references.

###### `$vendor{<macro-name>}`

An extension point for vendors to insert their own macros.

CMake will not be able to use presets which have a `$vendor{<macro-name>}` macro, and effectively ignores such presets. However, it will still be able to use other presets from the same file.

CMake does not make any attempt to interpret `$vendor{<macro-name>}` macros.

## References

- https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#id12