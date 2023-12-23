# Package Preset Format

##### `name`

- type: `string`
- required: yes

Unique name among package presets.

##### `hidden`

- type: bool
- required: no

Specifies whether or not a preset should be hidden.

See [[preset-hiding]].

##### `inherits`

- type: [`string`]() or [`array`]() of [`string`]()
- required: no

Name or names of presets to inherit from.

See [[preset-inheritance]].

##### `condition`

- type: [`object`]() of [[preset-condition]]
- required: no

Specifies if the preset should be enabled.

##### `vendor`

- type: [`object`]() of [[preset-vendor-map]]
- required: no

##### `displayName`

- type: [`string`]()
- required: no

A string with a human-friendly name of the preset.

##### `description`

- type: [`string`]()
- required: no

A string with a human-friendly description of the preset.

##### `environment`

> [!todo]
> Review this.

- type: [`object`]
- required: no

A map of environment variables. The key is the variable name (which may not be an empty string), and the value is either `null` or a string representing the value of the variable. Each variable is set regardless of whether or not a value was given to it by the process's environment. 

This field supports macro expansion, and environment variables in this map may reference each other, and may be listed in any order, as long as such references do not cause a cycle (for example, `ENV_1` is `$env{ENV_2}`, `ENV_2` may not be `$env{ENV_1}`.) Environment variables are inherited through the `inherits` field, and the preset's environment will be the union of its own `environment` and the `environment` from all its parents. If multiple presets in this union define the same variable, the standard rules of `inherits` are applied. Setting a variable to `null` causes it to not be set, even if a value was inherited from another reset.

##### `configurePreset`

- type: [`string`]()
- required: yes

Specifies the name of a configure preset to associate with this package preset.

If `configurePreset` is not specified, it must be inherited from the inherits preset unless this preset is hidden.

##### `inheritConfigureEnvironment`

- type: [`bool`]()
- required: no
- default: `true`

If true, the environment variables from the associated configure preset are inherited after all inherited package preset environments, but before environment variables explicitly specified in this package preset.

##### `generators`

- type: [`array`]() of [`string`]()
- required: no

Generators for [cpack](programming/tools/cpack/cpack) to use.

##### `configurations`

- type: [`array`]() of [`string`]()
- required: no

Build configurations for [cpack](programming/tools/cpack/cpack) to package.

##### `variables`

> [!todo]
> Review this.

A map of variables to pass to [cpack](programming/tools/cpack/cpack), equivalent to [`-D`](https://cmake.org/cmake/help/latest/manual/cpack.1.html#cmdoption-cpack-D) arguments.

Each key is the name of a variable, and the value is the string to assign to that variable.

##### `configFile`

- type: [`string`]()
- required: no

The config file for [cpack](programming/tools/cpack/cpack) to use.

##### `output`

- type: [`object`]()
- required: no

Specifies output options.

Valid keys are:

- `debug`
  
  - type: [`bool`]()
  - required: no
  
  Specifies whether or not to print debug information.
  
  If `true`, equivalent to passing [`--debug`](https://cmake.org/cmake/help/latest/manual/cpack.1.html#cmdoption-cpack-debug) on the command line.

- `verbose`
  
  - type: [`bool`]()
  - required: no
  
  If `true`, equivalent to passing [`--verbose`](https://cmake.org/cmake/help/latest/manual/cpack.1.html#cmdoption-cpack-V) on the command line.

##### `packageName`

- type: [`string`]()
- required: no

Package name.

##### `packageVersion`

- type: [`string`]()
- required: no

Package version.
    
##### `packageDirectory`

- type: [`string`]()
- required: unknown

The directory in which to place the package.

##### `vendorName`

- type: [`string`]()
- required: no

Vendor name.

## References

- https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#id9