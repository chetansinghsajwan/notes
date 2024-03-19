# CMake Presets

CMake Presets are a way to store cmake configuration options.

CMake supports two main files, `CMakePresets.json` and `CMakeUserPresets.json`, that allow users to specify common configure options

`CMakePresets.json` and `CMakeUserPresets.json` live in the project's root directory.

They both are optional (though at least one must be present if [`--preset`](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-preset) is specified).

`CMakePresets.json` is meant to specify project-wide build details, while `CMakeUserPresets.json` is meant for developers to specify their own local build details.

`CMakePresets.json` may be checked into a version control system, and `CMakeUserPresets.json` should not be checked in

`CMakePresets.json` and `CMakeUserPresets.json` follow the [[preset-format]] format.

## References

- https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html
  
- https://dominikberner.ch/cmake-presets-best-practices
  
- https://learn.microsoft.com/en-us/cpp/build/cmake-presets-json-reference?view=msvc-170
