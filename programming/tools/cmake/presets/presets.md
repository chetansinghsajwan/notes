# CMake Presets

CMake Presets are a way to store cmake configuration options.

CMake supports two main files, `CMakePresets.json` and `CMakeUserPresets.json`, that allow users to specify common configure options

`CMakePresets.json` and `CMakeUserPresets.json` live in the project's root directory.

They both are optional (though at least one must be present if [`--preset`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-preset) is specified).

`CMakePresets.json` is meant to specify project-wide build details, while `CMakeUserPresets.json` is meant for developers to specify their own local build details.

`CMakePresets.json` may be checked into a version control system, and `CMakeUserPresets.json` should not be checked in

See [[preset-format]].

---
##### Preset Condition

A [[preset-condition]] object to determine if the preset should be enabled.

This is allowed in preset files specifying version `3` or above.

##### Preset Vendor Maps

A map containing vendor-specific information.

CMake does not interpret the contents of this field except to verify that it is a map if it does exist.

More on [[preset-vendor-map]].

##### Preset Environment Setup

A map of environment variables.

The key is the variable name, and the value is either `null` or a string representing the value of the variable.

Environment variables in this map may reference each other, and may be listed in any order, as long as such references do not cause a cycle (for example, if `ENV_1` is `$env{ENV_2}`, `ENV_2` may not be `$env{ENV_1}`.)

This field supports [[macro-expansion]].

Environment variables are inherited through the `inherits` field, and the preset's environment will be the union of its own `environment` and the `environment` from all its parents. If multiple presets in this union define the same variable, the standard rules of `inherits` are applied.

Setting a variable to `null` causes it to not be set, even if a value was inherited from another preset.

## References

- https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html
  
- https://dominikberner.ch/cmake-presets-best-practices
  
- https://learn.microsoft.com/en-us/cpp/build/cmake-presets-json-reference?view=msvc-170