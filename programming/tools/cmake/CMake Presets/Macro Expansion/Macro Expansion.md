- Macros are recognized in the form `$<macro-namespace>{<macro-name>}`.
- Macros are evaluated in the context of the preset being used, even if the macro is in a field that was inherited from another preset.
    
    For example, if the `Base` preset sets variable `PRESET_NAME` to `${presetName}`, and the `Derived` preset inherits from `Base`, `PRESET_NAME` will be set to `Derived`.
    
- It is an error to not put a closing brace at the end of a macro name. For example, `${sourceDir` is invalid.
- A dollar sign (`$`) followed by anything other than a left curly brace (`{`) with a possible namespace is interpreted as a literal dollar sign.
Recognized macros include:
1. **`${sourceDir}`**
    
    Path to the project source directory (i.e. the same as [[CMake Variables]]).
    
2. **`${sourceParentDir}`**
    
    Path to the project source directory's parent directory.
    
3. **`${sourceDirName}`**
    
    The last filename component of `${sourceDir}`.
    
    For example, if `${sourceDir}` is `/path/to/source`, this would be `source`.
    
4. **`${presetName}`**
    
    Name specified in the preset's `name` field.
    
5. **`${generator}`**
    
    Generator specified in the preset's `generator` field.
    
    - For build and test presets, this will evaluate to the generator specified by `configurePreset`.
6. `${hostSystemName}`The name of the host operating system. Contains the same value as [`CMAKE_HOST_SYSTEM_NAME`](https://cmake.org/cmake/help/latest/variable/CMAKE_HOST_SYSTEM_NAME.html#variable:CMAKE_HOST_SYSTEM_NAME). This is allowed in preset files specifying version `3` or above.
7. `${fileDir}`Path to the directory containing the preset file which contains the macro. This is allowed in preset files specifying version `4` or above.
8. `${dollar}`A literal dollar sign (`$`).
9. `${pathListSep}`Native character for separating lists of paths, such as `:` or `;`. For example, by setting `PATH` to `/path/to/ninja/bin${pathListSep}$env{PATH}`, `${pathListSep}` will expand to the underlying operating system's character used for concatenation in `PATH`.  
    This is allowed in preset files specifying version `5` or above.
10. `$env{<variable-name>}`Environment variable with name `<variable-name>`. The variable name may not be an empty string. If the variable is defined in the `environment` field, that value is used instead of the value from the parent environment. If the environment variable is not defined, this evaluates as an empty string. Note that while Windows environment variable names are case-insensitive, variable names within a preset are still case-sensitive. This may lead to unexpected results when using inconsistent casing. For best results, keep the casing of environment variable names consistent.
11. `$penv{<variable-name>}`Similar to `$env{<variable-name>}`, except that the value only comes from the parent environment, and never from the `environment` field. This allows you to prepend or append values to existing environment variables. For example, setting `PATH` to `/path/to/ninja/bin:$penv{PATH}` will prepend `/path/to/ninja/bin` to the `PATH` environment variable. This is needed because `$env{<variable-name>}` does not allow circular references.
12. `$vendor{<macro-name>}`An extension point for vendors to insert their own macros. CMake will not be able to use presets which have a `$vendor{<macro-name>}` macro, and effectively ignores such presets. However, it will still be able to use other presets from the same file. CMake does not make any attempt to interpret `$vendor{<macro-name>}` macros. However, to avoid name collisions, IDE vendors should prefix `<macro-name>` with a very short (preferably <= 4 characters) vendor identifier prefix, followed by a `.`, followed by the macro name. For example, the Example IDE could have `$vendor{xide.ideInstallDir}`.