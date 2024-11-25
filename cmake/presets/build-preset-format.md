# Build Preset Format

###### `name`

- type: `string`
- required: yes

Unique name to identify this preset among other build presets.
  
###### `hidden`

- type: `bool`
- required: no

Specifies whether or not this preset should be hidden.

See [[preset-hiding]].

###### `inherits`

- type: `string` | `[string]`
- required: no

Name of preset or names of presets to inherit from.

See [[preset-inheritance]].

###### `condition`

- type: `object` of [[preset-condition]]
- required: no
- since: version 3

Determines whether or not the preset should be enabled.

###### `vendor`

- type: `map`
- required: no

A map of type [[preset-vendor-map]].

###### `displayName`

- type: `string`
- required: no

A string with a human-friendly name of the preset.

###### `description`

- type: `string`
- required: no

A string with a human-friendly description of the preset.
  
###### `environment`

- type: [`object`] of [[environment-map]]
- required: no

A map of environment variables.

###### `configurePreset`

- type: `string`
- required: yes

A string specifying the name of a [[configure-preset]] to associate with this build preset.

If `configurePreset` is not specified, it must be inherited from the inherits preset unless this preset is hidden.

###### `inheritConfigureEnvironment`

- type: `bool`
- required: no
- default: `true`

If true, the environment variables from the associated configure preset are inherited after all inherited build preset environments, but before environment variables explicitly specified in this build preset.

###### `jobs`

- type: `int`
- required: no

Equivalent to passing [`--parallel`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-build-j) on the command line.

###### `targets`

- type: `string` or `[string]`
- required: no

Equivalent to passing [`--target`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-build-t) on the command line.

This field supports [[macro-expansion]].

###### `configuration`

- type: `string`
- required: no

Equivalent to passing [`--config`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-build-config) on the command line.
  
###### `cleanFirst`

- type: `bool`
- required: no
- default: `false`

If true, equivalent to passing [`--clean-first`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-build-clean-first) on the command line.

###### `resolvePackageReferences`

- type: `string`
- required: no
- since: version 4

Specifies the package resolve mode.

Valid values are:

- `on`
- `off`
- `only`

Learn more in official docs.

###### `verbose`

- type: [`bool`]()
- required: no

If true, equivalent to passing [`--verbose`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-build-v) on the command line.

###### `nativeToolOptions`

- type: [`array`]() of [`string`]()
- required: no

Equivalent to passing options after `--` on the command line.

The array values support macro expansion.

## References

- https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#id7