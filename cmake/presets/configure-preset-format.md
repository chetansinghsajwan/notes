# Configure Preset Format

###### `name`

- type: [`string`]()
- required: yes

Unique name to identify this preset among other configure presets.

###### `displayName`

- type: [`string`]()
- required: no

A human-friendly name of the preset.

###### `description`

- type: [`string`]()
- required: no

A human-friendly description of the preset.

###### `hidden`

- type: [`bool`]()
- required: no

A boolean specifying whether or not this preset should be hidden.

See [[preset-hiding]].

###### `inherits`

- type: [`string`]() | [`[string]`]()
- required: no

Name of preset or names of presets to inherit from.

See [[preset-inheritance]].

###### `condition`

- type: [`object`]() of [preset-condition]()
- required: no
- since: version 3

Determines if the preset should be enabled.

###### `vendor`

- type: [`object`]() of [[preset-vendor-map]]
- required: no

A map containing vendor-specific information.

###### `generator`

- type: [`string`](json/data-types/string.md)
- required: until version 2

Generator to use for the preset.

Until version `2`, if `generator` is not specified, it must be inherited from the`inherits` preset unless this preset is `hidden`.

Since version `3`, this field may be omitted to fall back to regular generator discovery procedure.

Value must be one from [cmake-generators](generators.md).

> [!note]
> Not sure about the above statement.

> [!todo]
> Verify the above statement.

###### `architecture`, `toolset`

> [!todo]
> Review this.

- type: [`string`](json/data-types/string.md)
- required: no

Represents the platform and toolset, respectively, for [`generators`](https://cmake.org/cmake/help/latest/manual/cmake-generators.7.html#manual:cmake-generators(7)) that support them.  

See [`cmake -A`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-A) option for possible values for `architecture` and [`cmake -T`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-T) for `toolset`.  

Each may be either a string or an object with the following fields:
  
  - `value`
    
    An optional string representing the value.
  
  - `strategy`
    
    An optional string telling CMake how to handle the `architecture` or `toolset` field.

    Valid values are:
    
    - `set`
      
      Set the respective value. This will result in an error for generators that do not support the respective field.
    
    - `external`
      
      Do not set the value, even if the generator supports it. This is useful if, for example, a preset uses the Ninja generator, and an IDE knows how to set up the Visual C++ environment from the `architecture` and `toolset` fields. In that case, CMake will ignore the field, but the IDE can use them to set up the environment before invoking CMake.  

    If no `strategy` field is given, or if the field uses the string form rather than the object form, the behavior is the same as `"set"`.

###### `toolchainFile`

- type: [`string`](json/data-types/string.md)
- required: no
- since: version 3

Path to the toolchain file.

If a relative path is specified, it is calculated relative to the build directory, and if not found, relative to the source directory.

This field takes precedence over any [`CMAKE_TOOLCHAIN_FILE`](https://cmake.org/cmake/help/latest/variable/CMAKE_TOOLCHAIN_FILE.html#variable:CMAKE_TOOLCHAIN_FILE "CMAKE_TOOLCHAIN_FILE") value.

This field supports [[macro-expansion]].

###### `binaryDir`

- type: [`string`]()
- required: until version 2

Path to the output binary directory.

If a relative path is specified, it is calculated relative to the source directory.

Until version `2`, if `binaryDir` is not specified, it must be inherited from the `inherits` preset unless this preset is `hidden`.

This field supports [[macro-expansion]].

###### `cmakeExecutable`

- type: [`string`]()
- required: no

Path to the CMake executable to use for this preset.

This is reserved for use by IDEs, and is not used by CMake itself.

IDEs that use this field should expand any macros in it.

###### `cacheVariables`

- type: [`object`]
- required: no

A map of cache variables.

The key is the variable name.

The value is either:

- `null`

- `bool`
  
  Can also be written as `"TRUE"` or `"FALSE"`.

- `string`
  
  This supports [[macro-expansion]].

- [`object`]()
  
  This conatins the following fields:
  
  - `type`
    
    - type: [`string`]()
    - required: yes
    
    A string representing the type of the variable.

  - `value`
    
    - type: [`string`]
    - required: yes
    
    Value of the variable.
    
    Same rules as above.

Cache variables are inherited through the `inherits` field.

The preset's variables will be the union of its own `cacheVariables` and the `cacheVariables` from all its parents.

If multiple presets in this union define the same variable, the standard rules of `inherits` are applied.

Setting a variable to `null` causes it to not be set, even if a value was inherited from another preset.

###### `environment`

- type: [`object`]() of [[environment-map]]
- required: no

A map of environment variables.

###### `warnings`

- type: [`object`]()
- required: no

Specifies the warnings to enable. The object contains the following fields:

- `dev`
    
    - type: [`bool`]()
    - required: no
    
    Equivalent to passing [`-Wdev`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-Wdev) or [`-Wno-dev`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-Wno-dev) on the command line.

- `deprecated`
    
    - type: [`bool`]()
    - required: no
    
    Equivalent to passing [`-Wdeprecated`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-Wdeprecated) or[`-Wno-deprecated`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-Wno-deprecated) on the command line.

- `uninitialized`
    
    - type: [`bool`]()
    - required: no
    
    Setting this to `true` is equivalent to passing [`--warn-uninitialized`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-warn-uninitialized) on the command line.

- `unusedCli`
    
    - type: [`bool`]()
    - required: no
    
    Setting this to `false` is equivalent to passing [`--no-warn-unused-cli`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-no-warn-unused-cli) on the command line.

- `systemVars`
    
    - type: [`bool`]()
    - required: no
    
    Setting this to `true` is equivalent to passing [`--check-system-vars`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-check-system-vars) on the command line.

###### `errors`

- type: [`object`]()
- required: no

Specifies the errors to enable. The object may contain the following fields:

- `dev`
    
    - type: [`bool`]()
    - required: no
    
    Treats `warnings.dev` as errors. If `warnings.dev` is `false`, this has no effect.

- `deprecated`
    
    - type: [`bool`]()
    - required: no
    
    Treats `warnings.deprected` as errors. If `warnings.deprected` is `false`, this has no effect.

###### `debug`

- type: [`object`]()
- required: no

An object specifying debug options. The object contains the following fields:

- `output`
    
    - type: [`bool`]()
    - requried: no
    
    Setting this to `true` is equivalent to passing [`--debug-output`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-debug-output) on the command line.

- `tryCompile`
    
    - type: [`bool`]()
    - requried: no
    
    Setting this to `true` is equivalent to passing [`--debug-trycompile`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-debug-trycompile) on the command line.

- `find`
    
    - type: [`bool`]()
    - requried: no
    
    Setting this to `true` is equivalent to passing [`--debug-find`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-debug-find) on the command line.

###### `trace`

An object specifying trace options. The object may contain the following fields:

- `mode`
    
    - type: [`string`]()
    - required: no
    
    Specifies the trace mode. Valid values are:
    
    - `"on"`
        
        Equivalent to passing [`--trace`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-trace) on the command line.
    
    - `"off"`
        
        A trace of all calls will not be printed.
    
    - `"expand"`
        
        Equivalent to passing [`--trace-expand`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-trace-expand) on the command line.

- `format`
    
    - type: [`string`]()
    - required: no
    - default: `"human"`
    
    Specifies the format output of the trace. Valid values are:
    
    - `"human"`
        
        Equivalent to passing [`--trace-format=human`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-trace-format) on the command line.
    
    - `json-v1`
        
        Equivalent to passing [`--trace-format=json-v1`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-trace-format) on the command line.

- `source`
    
    - type: [`string`]() or [`array`]() of [`string`]()
    - required: no
    
    Equivalent to passing [`--trace-source`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-trace-source) on the command line.

- `redirect`
    
    - type: [`string`]()
    - required: no
    
    Equivalent to passing [`--trace-redirect`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-trace-redirect) on the command line.

## References

- https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#id6