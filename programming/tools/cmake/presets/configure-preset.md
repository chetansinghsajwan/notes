# Configure Preset

1. **`name`**
    
    - type: [`string`]()
    - required: yes
    
    Unique name to identify this preset among other configure presets.
    
2. **`displayName`**
    
    - type: [`string`]()
    - required: no
    
    A human-friendly name of the preset.
    
3. **`description`**
    
    - type: [`string`]()
    - required: no
    
    A human-friendly description of the preset.
    
4. **`hidden`**
    
    - type: [`bool`]()
    - required: no
    
    A boolean specifying whether or not this preset should be hidden.
    
    - If the preset is hidden, it cannot be used in the [[CMake Options]] argument.
    - `hidden` presets are intended to be used as a base for other presets to inherit via the `inherits` field.
    
5. **`inherits`**
    
    - type: [`string`]() | [`[string]`]()
    - required: no
    
    Name of preset or names of presets to inherit from.
    
    - The preset will inherit all of the fields from the `inherits` presets by default (except `name`, `hidden`, `inherits`,`description`, and `displayName`), and can override them as desired.
    - If multiple `inherits` presets provide conflicting values for the same field, the earlier preset in the `inherits` array will be preferred.
    - A preset can only inherit from another preset that is defined in the same file or in one of the files it includes (directly or indirectly).
    - Presets in `CMakePresets.json` may not inherit from presets in `CMakeUserPresets.json`.
    
6. **`condition`**
    
    - type: [`object`]() of [condition]()
    
    A [[programming/tools/cmake/presets/condition]] object to determine if the preset should be enabled.
    
    - This is allowed in preset files specifying version `3` or above.
    
7. **`vendor`**
    
    - type: [`object`]() of [[vendor-map]]
    - required: no
    
    A map containing vendor-specific information.
    
    - CMake does not interpret the contents of this field except to verify that it is a map if it does exist.
    - More on [[vendor-map]].
    
8. **`generator`**
    
    - type: [`string`]()
    - required: yes | no
    
    Generator to use for the preset.
    
    - It is required in version `2` or below. If `generator` is not specified, it must be inherited from the`inherits` preset (unless this preset is `hidden`).
    
    - In version `3`or above, this field may be omitted to fall back to regular generator discovery procedure.
    
1. **`architecture`**, **`toolset`**
    
    Optional fields representing the platform and toolset, respectively, for [`**generators**`](https://cmake.org/cmake/help/latest/manual/cmake-generators.7.html#manual:cmake-generators(7)) that support them.  
    See [`**cmake -A**`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-A) option for possible values for `architecture` and [`**cmake -T**`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-T) for `toolset`.  
    
    Each may be either a string or an object with the following fields:`value`An optional string representing the value.`strategy`An optional string telling CMake how to handle the `architecture` or `toolset` field.
    
    Valid values are:
    
    - `"set"`Set the respective value. This will result in an error for generators that do not support the respective field.
    
    - `"external"`Do not set the value, even if the generator supports it. This is useful if, for example, a preset uses the Ninja generator, and an IDE knows how to set up the Visual C++ environment from the `architecture` and `toolset` fields. In that case, CMake will ignore the field, but the IDE can use them to set up the environment before invoking CMake.  
    
    If no `strategy` field is given, or if the field uses the string form rather than the object form, the behavior is the same as `"set"`.
    
10. **`toolchainFile`**
    
    - type: [`string`]()
    - reqired: no
    
    Path to the toolchain file.
    
    - If a relative path is specified, it is calculated relative to the build directory, and if not found, relative to the source directory.
    - This field takes precedence over any [[CMake Variables]] value.
    - This field supports [[macro-expansion]].
    - It is allowed in preset files specifying version `3` or above.

11. **`binaryDir`**
    
    - type: [`string`]()
    - required: yes | no
    
    Path to the output binary directory.
    
    - If a relative path is specified, it is calculated relative to the source directory.
    - It is required in version `2` or below. If `binaryDir` is not specified, it must be inherited from the `inherits` preset (unless this preset is `hidden`).
    - This field supports [[macro-expansion]].
    - In version `3` or above, this field may be omitted.

12. **`cmakeExecutable`**
    
    - type: [`string`]()
    - required: no
    
    Path to the CMake executable to use for this preset.
    
    - This is reserved for use by IDEs, and is not used by CMake itself.
    - IDEs that use this field should expand any macros in it.

13. **`cacheVariables`**
    
    - type: `[string: (null | bool | object)]`
    
    A map of cache variables.
    
    - The key is the variable name.
    - The value is either:
        - `null`
        - `bool`, can also be written as `"TRUE"` or `"FALSE"`.
        - `string` representing the value of the variable. This supports [[macro-expansion]].
        - An object with the following fields:
            - **`type`****:** **`string`**
                
                A string representing the type of the variable.
                
            - `value` **(required)**
                
                Same rules as above.
                
    - Cache variables are inherited through the `inherits` field.
    - The preset's variables will be the union of its own `cacheVariables` and the `cacheVariables` from all its parents.
    - If multiple presets in this union define the same variable, the standard rules of `inherits` are applied.
    - Setting a variable to `null` causes it to not be set, even if a value was inherited from another preset.
14. **`environment`** **:** **`[string: string]`**
    
    A map of environment variables.
    
    - The key is the variable name, and the value is either `null` or a string representing the value of the variable.
    - Environment variables in this map may reference each other, and may be listed in any order, as long as such references do not cause a cycle (for example, if `ENV_1` is `$env{ENV_2}`, `ENV_2` may not be `$env{ENV_1}`.)
    - This field supports [[macro-expansion]].
    - Environment variables are inherited through the `inherits` field, and the preset's environment will be the union of its own `environment` and the `environment` from all its parents. If multiple presets in this union define the same variable, the standard rules of `inherits` are applied.
    - Setting a variable to `null` causes it to not be set, even if a value was inherited from another preset.
    
15. **`warnings`****:** **`{}`**
    
    An object specifying the warnings to enable.
    
    The object contains the following fields:
    
    - **`dev`****:** `**bool**`
        
        Equivalent to passing [`-Wdev`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-Wdev) or [`-Wno-dev`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-Wno-dev) on the command line. This may not be set to `false` if `errors.dev` is set to `true`.
        
    - `deprecated`
        
        An optional boolean. Equivalent to passing [`-Wdeprecated`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-Wdeprecated) or[`-Wno-deprecated`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-Wno-deprecated) on the command line. This may not be set to `false` if `errors.deprecated` is set to `true`.
        
    - `uninitialized`
        
        An optional boolean. Setting this to `true` is equivalent to passing [`--warn-uninitialized`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-warn-uninitialized) on the command line.
        
    - `unusedCli`
        
        An optional boolean. Setting this to `false` is equivalent to passing [`--no-warn-unused-cli`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-no-warn-unused-cli) on the command line.
        
    - `systemVars`
        
        An optional boolean. Setting this to `true` is equivalent to passing [`--check-system-vars`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-check-system-vars) on the command line.
        
16. `errors`
    
    An optional object specifying the errors to enable. The object may contain the following fields:
    
    - `dev`
        
        Treats `warnings.dev` as errors. If `warnings.dev` is `false`, this has no effect.
        
    - `deprecated`
        
        Treats `warnings.deprected` as errors. If `warnings.deprected` is `false`, this has no effect.
        
17. `debug`
    
    An optional object specifying debug options. The object contains the following fields:
    
    - `output`
        
        A boolean. Setting this to `true` is equivalent to passing [`--debug-output`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-debug-output) on the command line.
        
    - `tryCompile`
        
        A boolean. Setting this to `true` is equivalent to passing [`--debug-trycompile`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-debug-trycompile) on the command line.
        
    - `find`
        
        A boolean. Setting this to `true` is equivalent to passing [`--debug-find`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-debug-find) on the command line.
        
18. `trace`
    
    A object specifying trace options. The object may contain the following fields:
    
    - `mode`
        
        An optional string that specifies the trace mode. Valid values are:
        
        - `on`
            
            Causes a trace of all calls made and from where to be printed. Equivalent to passing [`--trace`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-trace) on the command line.
            
        - `off`
            
            A trace of all calls will not be printed.
            
        - `expand`
            
            Causes a trace with variables expanded of all calls made and from where to be printed. Equivalent to passing [`--trace-expand`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-trace-expand) on the command line.
            
    - `format`
        
        An optional string that specifies the format output of the trace. Valid values are:
        
        - `human`
            
            Prints each trace line in a human-readable format.  
            This is the default format. Equivalent to passing  
            [`--trace-format=human`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-trace-format) on the command line.
            
        - `json-v1`
            
            Prints each line as a separate JSON document. Equivalent to passing  
            [`--trace-format=json-v1`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-trace-format) on the command line.
            
    - `source`
        
        An optional array of strings representing the paths of source files to be traced. This field can also be a string, which is equivalent to an array containing one string. Equivalent to passing [`--trace-source`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-trace-source) on the command line.
        
    - `redirect`
        
        An optional string specifying a path to a trace output file. Equivalent to passing [`--trace-redirect`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-trace-redirect) on the command line.

## References

- https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#id6