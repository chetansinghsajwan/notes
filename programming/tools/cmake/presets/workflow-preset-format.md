# Workflow Preset Format

1. `name` **(required)**
    
    Unique name among workflow presets.
    
2. `displayName`
    
    A string with a human-friendly name of the preset.
    
3. `description`
    
    A string with a human-friendly description of the preset.
    
4. `vendor`
    
    A map containing vendor-specific information.
    
    - CMake does not interpret the contents of this field except to verify that it is a map  
        if it does exist.
    - More on [[vendor-map]].
    
    A map containing vendor-specific information.
    
    - CMake does not interpret the contents of this field except to verify that it is a map if it does exist.
    - More on [[vendor-map]].
    
5. `steps` **(required)**
    
    A array of objects describing the steps of the workflow.
    
    - The first step must be a configure preset, and all subsequent steps must be non-configure presets whose `configurePreset` field matches the starting configure preset.
    - Each object may contain the following fields:
        
        - `type` **(required)**
            
            A string representing the task type.
            
            - The first step must be `configure`. Subsequent steps must be either `build`, `test`, or `package`.
        - `name` **(required)**
            
            A string representing the name of the preset to run as this workflow step.

## References

- https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#id10