1. `name` **(required)**
    
    Unique name among package presets.
    
2. `hidden`
    
    A boolean specifying whether or not a preset should be hidden.
    
    - If a preset is hidden, it cannot be used in the [`--preset`](https://cmake.org/cmake/help/latest/manual/cpack.1.html#cmdoption-cpack-preset) argument  
        and does not have to have a valid `configurePreset`, even from  
        inheritance.
    - `hidden` presets are intended to be used as a base for  
        other presets to inherit via the `inherits` field.
3. `inherits`An optional array of strings representing the names of presets to inherit  
    from. This field can also be a string, which is equivalent to an array containing one string.  
    The preset will inherit all of the fields from the `inherits` presets by default (except `name`, `hidden`, `inherits`, `description`, and `displayName`), but can override them as desired. If multiple `inherits` presets provide conflicting values for the same field, the earlier preset in the `inherits` array will be preferred.
    - A preset can only inherit from another preset that is defined in the  
        same file or in one of the files it includes (directly or indirectly).  
        Presets in `CMakePresets.json` may not inherit from presets in  
        `CMakeUserPresets.json`.`condition`An optional [Condition](https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#condition) object.
4. `vendor`
    
    A map containing vendor-specific information. CMake does not  
    interpret the contents of this field except to verify that it is a map  
    if it does exist. However, it should follow the same conventions as the  
    root-level `vendor` field. If vendors use their own per-preset  
    `vendor` field, they should implement inheritance in a sensible manner  
    when appropriate.
    
5. `displayName`
    
    A string with a human-friendly name of the preset.
    
6. `description`
    
    A string with a human-friendly description of the preset.
    
7. `environment`
    
    A map of environment variables. The key is the variable name (which may not be an empty string), and the value is either `null` or a string representing the value of the variable. Each variable is set regardless of whether or not a value was given to it by the process's  
    environment. This field supports macro expansion, and environment variables in this map may reference each other, and may be listed in any order, as long as such references do not cause a cycle (for example, `ENV_1` is `$env{ENV_2}`, `ENV_2` may not be `$env{ENV_1}`.) Environment variables are inherited through the `inherits` field, and the preset's environment will be the union of its own `environment` and the `environment` from all its parents. If multiple presets in  
    this union define the same variable, the standard rules of `inherits` are applied. Setting a variable to `null` causes it to not be set, even if a value was inherited from another reset.
    
8. `configurePreset`
    
    A string specifying the name of a configure preset to associate with this package preset. If `configurePreset` is not specified, it must be inherited from the inherits preset (unless this  
    preset is hidden). The build directory is inferred from the configure preset, so packaging will run in the same `binaryDir` that the configuration did and build did.
    
9. `inheritConfigureEnvironment`
    
    A boolean that defaults to true. If true, the environment variables from the associated configure preset are inherited after all inherited package preset environments, but before environment variables explicitly specified in this package preset.
    
10. `generators`
    
    An array of strings representing generators for CPack to use.
    
11. `configurations`
    
    An array of strings representing build configurations for CPack to package.
    
12. `variables`
    
    A map of variables to pass to CPack, equivalent to [`-D`](https://cmake.org/cmake/help/latest/manual/cpack.1.html#cmdoption-cpack-D) arguments. Each key is the name of a variable, and the value is the string to assign to that variable.`configFile`An optional string representing the config file for CPack to use.`output`An optional object specifying output options. Valid keys are:`debug`An optional boolean specifying whether or not to print debug information.  
    A value of `true` is equivalent to passing [`--debug`](https://cmake.org/cmake/help/latest/manual/cpack.1.html#cmdoption-cpack-debug) on the command line.`verbose`An optional boolean specifying whether or not to print verbosely. A value of `true` is equivalent to passing [`--verbose`](https://cmake.org/cmake/help/latest/manual/cpack.1.html#cmdoption-cpack-V) on the command line.`packageName`An optional string representing the package name.
    
13. `packageVersion`
    
    A string representing the package version.
    
14. `packageDirectory`
    
    A string representing the directory in which to place the package.
    
15. `vendorName`
    
    A string representing the vendor name.