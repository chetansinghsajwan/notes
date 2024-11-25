# Preset Format

The root object contains these fields:

###### `version`

- type: [`number`](json/data-types/number.md)
- required: yes

A integer representing the version of the JSON schema.

The supported versions are:

- `1`: Added in cmake version 3.19
- `2`: Added in cmake version 3.20
- `3`: Added in cmake version 3.21
- `4`: Added in cmake version 3.23
- `5`: Added in cmake version 3.24
- `6`: Added in cmake version 3.25
- `7`: Added in cmake version 3.27

###### `cmakeMinimumRequired`

- type: [`object`](json/data-types/object.md)
- required: no

Object containing minimum version of cmake required to build this project.

- `major`
  
  - type: [`integer`](json/data-types/number.md)
  - required: no

- `minor`
  
  - type: [`integer`](json/data-types/number.md)
  - required: no

- `patch`
  
  - type: [`integer`](json/data-types/number.md)
  - required: no

###### `include`

- type: [`array`](array.md) of [`string`](json/data-types/string.md)
- required: no
- since: version 4

List of filepaths to include. If the filepaths are not absolute, they are considered relative to the current file.

See [[preset-include-rules]].

###### `vendor`

- type: [`object`]() of [[preset-vendor-map]]
- required: no

A map containing vendor-specific information.

###### `configurePresets`

- type: [`array`]() of [[configure-preset]]
- required: no
- since: version 1

###### `buildPresets`

- type: [`array`]() of [[build-preset]]
- required: no
- since: version 2

###### `testPresets`

- type: [`array`]() of [[test-preset]]
- required: no
- since: version 2

###### `packagePresets`

- type: [`array`]() of [[package-preset]]
- required: no
- since: version 6

###### `workflowPresets`

- type: [`array`]() of [[workflow-preset]]
- required: no
- since: version 6

## Related

- [[preset-example]]

## References

- https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#id4