# Preset Inheritance

A preset will inherit all of the fields from the `inherits` presets by default (except `name`, `hidden`, `inherits`,`description`, and `displayName`), and can override them as desired.

If multiple `inherits` presets provide conflicting values for the same field, the earlier preset in the `inherits` array will be preferred.

A preset can only inherit from another preset that is defined in the same file or in one of the files it includes (directly or indirectly).

Presets in `CMakePresets.json` may not inherit from presets in `CMakeUserPresets.json`.

## References

- https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html