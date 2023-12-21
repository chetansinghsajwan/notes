# Build Preset Format

###### `name`

- type: `string`
- required: yes

Unique name to identify this preset among other build presets.
  
###### `hidden`

- type: `bool`
- required: no

A boolean specifying whether or not this preset should be hidden.

If the preset is hidden, it cannot be used in the [[CMake Options]] argument.

Hidden presets are intended to be used as a base for other presets to inherit via the `inherits` field.

###### `inherits`

- type: `string` | `[string]`
- required: no

Name of preset or names of presets to inherit from.

The preset will inherit all of the fields from the `inherits` presets by default (except `name`, `hidden`, `inherits`,`description`, and `displayName`), and can override them as desired.

If multiple `inherits` presets provide conflicting values for the same field, the earlier preset in the `inherits` array will be preferred.

A preset can only inherit from another preset that is defined in the same file or in one of the files it includes (directly or indirectly).

Presets in `CMakePresets.json` may not inherit from presets in `CMakeUserPresets.json`.

###### `condition`

- type: `object` of `condition`
- required: no

###### `vendor`

- type: `map`
- required: no

A map of type [[vendor-map]].
 
###### `displayName`

- type: `string`
- required: no

A string with a human-friendly name of the preset.

###### `description`

- type: `string`
- required: no

A string with a human-friendly description of the preset.
  
###### `environment`

- type: `map`
- required: no

A map of environment variables.

Environment variables in this map may reference each other, and may be listed in any order, as long as such references do not cause a cycle.

This field supports [[macro-expansion]].

Environment variables are inherited through the `inherits` field, and the preset's environment will be the union of its own `environment` and the `environment` from all its parents. If multiple presets in this union define the same variable, the standard rules of `inherits` are applied.

Setting a variable to `null` causes it to not be set, even if a value was inherited from another preset.
  
###### `configurePreset`

- type: `string`
- required: yes

A string specifying the name of a [[configure-preset-format]] to associate with this build preset.

If `configurePreset` is not specified, it must be inherited from the inherits preset (unless this preset is hidden).

The build directory is inferred from the configure preset, so the build will take place in the same `binaryDir` that the configuration did.

###### `inheritConfigureEnvironment`

- type: `bool`
- required: no

An optional boolean that defaults to true. If true, the environment variables from the associated configure preset are inherited after all inherited build preset environments, but before environment variables explicitly specified in this build preset.

###### `jobs`

- type: `int`
- required: no

An integer. Equivalent to passing [`--parallel`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-build-j) or `-j` on the command line.

###### `targets`

- type: `string` or `[string]`
- required: no

A string or array of strings. Equivalent to passing [`--target`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-build-t) or `-t` on the command line.

This field supports macro expansion.

###### `configuration`

- type: `string`
- required: no

A string. Equivalent to passing [`--config`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-build-config) on the command line.
  
###### `cleanFirst`

- type: `bool`
- required: no

A bool. If true, equivalent to passing [`--clean-first`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-build-clean-first) on the command line.
    
###### `resolvePackageReferences`

- type: `string`
- required: no

A string that specifies the package resolve mode.

Package references are used to define dependencies to packages from external package managers. Currently only NuGet in combination with the Visual Studio generator is supported. If there are no targets that define package references, this option does nothing.

Valid values are:

- `on`
  
  Causes package references to be resolved before attempting a build.
  
- `off`

  Package references will not be resolved. Note that this may cause errors in some build environments, such as .NET SDK style projects.

- `only`
  
  Only resolve package references, but do not perform a build.
  
> [!note]
> 
> The command line parameter[`--resolve-package-references`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-build-resolve-package-references) will take priority over this setting. If the command line parameter is not provided and this setting is not specified, an environment-specific cache variable will be evaluated to decide, if package restoration should be performed.  
> 
> When using the Visual Studio generator, package references are defined using the [`VS_PACKAGE_REFERENCES`](https://cmake.org/cmake/help/latest/prop_tgt/VS_PACKAGE_REFERENCES.html#prop_tgt:VS_PACKAGE_REFERENCES) property. Package references are restored using NuGet. It can be disabled by setting the `CMAKE_VS_NUGET_PACKAGE_RESTORE` variable to `OFF`. This can also be done from within a configure preset.`verbose`An optional bool. If true, equivalent to passing [`--verbose`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-build-v) on the command line.`nativeToolOptions`An optional array of strings. Equivalent to passing options after `--` on the command line. The array values support macro expansion.

## References

- https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#id7