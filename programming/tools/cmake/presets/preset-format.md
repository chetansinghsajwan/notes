# Preset Format

The root object contains these fields:

###### `version`

- type: [`number`](programming/languages/json/data-types/number)
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

- type: [`object`](programming/languages/json/data-types/object)
- required: no

Object containing minimum version of cmake required to build this project.

- `major`
  
  - type: [`integer`](programming/languages/json/data-types/number)
  - required: no

- `minor`
  
  - type: [`integer`](programming/languages/json/data-types/number)
  - required: no

- `patch`
  
  - type: [`integer`](programming/languages/json/data-types/number)
  - required: no

###### `include`

- type: [`array`](programming/languages/json/data-types/array) of [`string`](programming/languages/json/data-types/string)
- required: no
- since: version 4

List of filepaths to include. If the filepaths are not absolute, they are considered relative to the current file.

See [[preset-include-rules]].

###### `vendor`

> [!todo]
> 
> Review this.

- type: [`map`]()
- required: no

A map containing vendor-specific information. CMake does not interpret the contents of this field except to verify that it is a map if it does exist.

However, the keys should be a vendor-specific domain name followed by a `/`-separated path.

For example, the Example IDE 1.0 could use `example.com/ExampleIDE/1.0`. The value of each field can be anything desired by the vendor, though will typically be a map.

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