# Preset Format

The files are a json document with an object as the root. The root object contains these fields:

1. `version`
    
    - type: [`number`]()
    - required: yes
    
    A integer representing the version of the JSON schema.
    
    The supported versions are:
    
    - `1`: New in version 3.19
    - `2`: New in version 3.20
    - `3`: New in version 3.21
    - `4`: New in version 3.23
    - `5`: New in version 3.24
    - `6`: New in version 3.25
    - `7`: New in version 3.27

2. `cmakeMinimumRequired`
    
    - type: [`object`]()
    - required: no
    
    Object containing minimum version of cmake required to build this project.
    
    - `major`
    - `minor`
    - `patch`

3. `include`
    
    - type: [`string`]()
    - required: no
    - since: version 4
    
    An array of strings representing files to include. If the filenames are not absolute, they are considered relative to the current file.
    
    See [[preset-include]] for discussion of the constraints on included files.

4. `vendor`
    
    - type: [`map`]()
    - required: no
    
    A map containing vendor-specific information. CMake does not interpret the contents of this field except to verify that it is a map if it does exist.
    
    However, the keys should be a vendor-specific domain name followed by a `/`-separated path.
    
    For example, the Example IDE 1.0 could use `example.com/ExampleIDE/1.0`. The value of each field can be anything desired by the vendor, though will typically be a map.

5. `configurePresets`
    
    - type: [`array`]() of [configure-preset]()
    - required: no
    - since: version 1
    
    An array of [[configure-preset]] objects.  

6. `buildPresets`
    
    - type: [`array`]() of [build-preset]()
    - required: no
    - since: version 2
    
    An array of [[build-preset]] objects.

7. `testPresets`
    
    - type: [`array`]() of [test-preset]()
    - required: no
    - since: version 2
    
    An array of [[test-preset]] objects.

8. `packagePresets`
    
    - type: [`array`]() of [package-preset]()
    - required: no
    - since: version 6
    
    An array of [[package-preset]] objects.

9. `workflowPresets`
    
    - type: [`array`]() of [workflow-preset]()
    - required: no
    - since: version 6
    
    An array of [[workflow-preset]] objects.

## References

- https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#id4