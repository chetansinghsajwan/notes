# Workflow Preset Format

##### `name`

- type: [`string`]()
- required: yes

Unique name among workflow presets.

##### `displayName`

- type: [`string`]()
- required: no

A human-friendly name of the preset.

##### `description`

- type: [`string`]()
- required: no

A human-friendly description of the preset.

##### `vendor`

- type: [`object`]() of [[preset-vendor-map]]
- required: no

A map containing vendor-specific information.

##### `steps`

- type: [`array`]() of [`object`]()
- required: yes

A array of objects describing the steps of the workflow.

The first step must be a configure preset, and all subsequent steps must be non-configure presets whose `configurePreset` field matches the starting configure preset.

Each object may contain the following fields:

- `type`

    - type: [`string`]()
    - required: yes

    A string representing the task type.
        
    The first step must be `configure`. Subsequent steps must be either `build`, `test`, or `package`.

- `name`
    
    - type: [`string`]()
    - required: yes

    A string representing the name of the preset to run as this workflow step.

## References

- https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#id10