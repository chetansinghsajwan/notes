CMake Presets are a way to store cmake configuration options.
- CMake supports two main files, `CMakePresets.json` and `CMakeUserPresets.json`, that allow users to specify common configure options
- `CMakePresets.json` and `CMakeUserPresets.json` live in the project's root directory.
- They both are optional (though at least one must be present if [`--preset`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-preset) is specified).
- `CMakePresets.json` is meant to specify project-wide build details, while `CMakeUserPresets.json` is meant for developers to specify their own local build details.
- `CMakePresets.json` may be checked into a version control system, and `CMakeUserPresets.json` should NOT be checked in
## Format
The files are a JSON document with an object as the root. The root object contains these fields:
1. `version` **(required)**
    
    A integer representing the version of the JSON schema.
    
    The supported versions are:
    
    - `1` - New in version 3.19
    - `2` - New in version 3.20
    - `3` - New in version 3.21
    - `4` - New in version 3.23
    - `5` - New in version 3.24
    - `6` - New in version 3.25
    - `7` - New in version 3.27
2. `cmakeMinimumRequired`
    
    Object containing minimum version of cmake required to build this project.
    
    - `major`
    - `minor`
    - `patch`
3. **`include`** → **`[string]`**
    
    An array of strings representing files to include. If the filenames are not absolute, they are considered relative to the current file.
    
    This is allowed in preset files specifying version `4` or above. See [Includes](https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#includes) for discussion of the constraints on included files.
    
4. **`vendor`** → **`map`**
    
    An optional map containing vendor-specific information. CMake does not interpret the contents of this field except to verify that it is a map if it does exist. However, the keys should be a vendor-specific domain name followed by a `/`-separated path. For example, the Example IDE 1.0 could use `example.com/ExampleIDE/1.0`. The value of each field can be anything desired by the vendor, though will typically be a map.
    
5. `configurePresets`
    
    An optional array of [Configure Preset](https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#configure-preset) objects.  
    This is allowed in preset files specifying version `1` or above.
    
6. `buildPresets`
    
    An optional array of [Build Preset](https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#build-preset) objects.  
    This is allowed in preset files specifying version `2` or above.
    
7. `testPresets`
    
    An optional array of [Test Preset](https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#test-preset) objects.  
    This is allowed in preset files specifying version `2` or above.
    
8. `packagePresets`
    
    An optional array of [Package Preset](https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#package-preset) objects.  
    This is allowed in preset files specifying version `6` or above.
    
9. `workflowPresets`
    
    An optional array of [Workflow Preset](https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#workflow-preset) objects.  
    This is allowed in preset files specifying version `6` or above.
    
---
[[Includes]]
[[Configure Preset]]
[[Build Preset]]
[[Test Preset]]
[[Package Preset]]
[[Workflow Preset]]
[[Condition]]
[[Macro Expansion]]
[[Vendor Maps]]
  
- **Preset Hiding**
    
    A boolean specifying whether or not this preset should be hidden.
    
    - If the preset is hidden, it cannot be used in the [[CMake Options]] argument.
    - `hidden` presets are intended to be used as a base for other presets to inherit via the `inherits` field.
    
- **Preset Inheritance**
    
    Name of preset or names of presets to inherit from.
    
    - The preset will inherit all of the fields from the `inherits` presets by default (except `name`, `hidden`, `inherits`,`description`, and `displayName`), and can override them as desired.
    - If multiple `inherits` presets provide conflicting values for the same field, the earlier preset in the `inherits` array will be preferred.
    - A preset can only inherit from another preset that is defined in the same file or in one of the files it includes (directly or indirectly).
    - Presets in `CMakePresets.json` may not inherit from presets in `CMakeUserPresets.json`.
    
- **Preset Condition**
    
    A [[Condition]] object to determine if the preset should be enabled.
    
    - This is allowed in preset files specifying version `3` or above.
    
- **Preset Vendor Maps**
    
    A map containing vendor-specific information.
    
    - CMake does not interpret the contents of this field except to verify that it is a map if it does exist.
    - More on [[Vendor Maps]].
    
- **Preset Environment Setup**
    
    A map of environment variables.
    
    - The key is the variable name, and the value is either `null` or a string representing the value of the variable.
    - Environment variables in this map may reference each other, and may be listed in any order, as long as such references do not cause a cycle (for example, if `ENV_1` is `$env{ENV_2}`, `ENV_2` may not be `$env{ENV_1}`.)
    - This field supports [[Macro Expansion]].
    - Environment variables are inherited through the `inherits` field, and the preset's environment will be the union of its own `environment` and the `environment` from all its parents. If multiple presets in this union define the same variable, the standard rules of `inherits` are applied.
    - Setting a variable to `null` causes it to not be set, even if a value was inherited from another preset.
    
## Example
```
{
  "version": 6,
  "cmakeMinimumRequired": {
    "major": 3,
    "minor": 23,
    "patch": 0
  },
  "include": [
    "otherThings.json",
    "moreThings.json"
  ],
  "configurePresets": [
    {
      "name": "default",
      "displayName": "Default Config",
      "description": "Default build using Ninja generator",
      "generator": "Ninja",
      "binaryDir": "${sourceDir}/build/default",
      "cacheVariables": {
        "FIRST_CACHE_VARIABLE": {
          "type": "BOOL",
          "value": "OFF"
        },
        "SECOND_CACHE_VARIABLE": "ON"
      },
      "environment": {
        "MY_ENVIRONMENT_VARIABLE": "Test",
        "PATH": "$env{HOME}/ninja/bin:$penv{PATH}"
      },
      "vendor": {
        "example.com/ExampleIDE/1.0": {
          "autoFormat": true
        }
      }
    },
    {
      "name": "ninja-multi",
      "inherits": "default",
      "displayName": "Ninja Multi-Config",
      "description": "Default build using Ninja Multi-Config generator",
      "generator": "Ninja Multi-Config"
    },
    {
      "name": "windows-only",
      "inherits": "default",
      "displayName": "Windows-only configuration",
      "description": "This build is only available on Windows",
      "condition": {
        "type": "equals",
        "lhs": "${hostSystemName}",
        "rhs": "Windows"
      }
    }
  ],
  "buildPresets": [
    {
      "name": "default",
      "configurePreset": "default"
    }
  ],
  "testPresets": [
    {
      "name": "default",
      "configurePreset": "default",
      "output": {"outputOnFailure": true},
      "execution": {"noTestsAction": "error", "stopOnFailure": true}
    }
  ],
  "packagePresets": [
    {
      "name": "default",
      "configurePreset": "default",
      "generators": [
        "TGZ"
      ]
    }
  ],
  "workflowPresets": [
    {
      "name": "default",
      "steps": [
        {
          "type": "configure",
          "name": "default"
        },
        {
          "type": "build",
          "name": "default"
        },
        {
          "type": "test",
          "name": "default"
        },
        {
          "type": "package",
          "name": "default"
        }
      ]
    }
  ],
  "vendor": {
    "example.com/ExampleIDE/1.0": {
      "autoFormat": false
    }
  }
}
```
---
# To Do
1. Should we write json type in the heading?
# References

> [!info] cmake-presets(7) — CMake 3.27.4 Documentation  
> Contents  
> [https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html](https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html)  

> [!info] Organizing CMake presets  
> CMake presets are a big help in how to configure CMake.  
> [https://dominikberner.ch/cmake-presets-best-practices/](https://dominikberner.ch/cmake-presets-best-practices/)  

> [!info] CMakeUserPresets.json  
> Schema reference for `CMakePresets.  
> [https://learn.microsoft.com/en-us/cpp/build/cmake-presets-json-reference?view=msvc-170](https://learn.microsoft.com/en-us/cpp/build/cmake-presets-json-reference?view=msvc-170)