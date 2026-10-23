The root object contains these fields:
### `version`
- type: `number`
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
### `cmakeMinimumRequired`
- type: `object`
- required: no
Object containing minimum version of cmake required to build this project.
- `major`
	- type: `integer`
	- required: no
- `minor`
	- type: `integer`
	- required: no
- `patch`
	- type: `integer`
	- required: no
### `include`
- type: `array` of `string`
- required: no
- since: version 4
List of filepaths to include. If the filepaths are not absolute, they are considered relative to the current file.
See [[Preset Include Rules]].
### `vendor`
- type: `object` of [[Preset Vendor Map]]
- required: no
A map containing vendor-specific information.
### `configurePresets`
- type: `array` of [[Configure Preset]]
- required: no
- since: version 1
### `buildPresets`
- type: `array` of [[Build Preset]]
- required: no
- since: version 2
### `testPresets`
- type: `array` of [[Test Preset]]
- required: no
- since: version 2
### `packagePresets`
- type: `array` of [[Package Preset]]
- required: no
- since: version 6
### `workflowPresets`
- type: `array` of [[Workflow Preset]]
- required: no
- since: version 6
## Related
- [[Preset Example]]
## References
- https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html\#id4