# Preset Include Rules

`CMakePresets.json` and `CMakeUserPresets.json` can include other files with the `include` field in file version `4` and later.

Files included by these files can also include other files.

If `CMakePresets.json` and `CMakeUserPresets.json` are both present, `CMakeUserPresets.json` implicitly includes `CMakePresets.json`, even with no `include` field.

If a preset file contains presets that inherit from presets in another file, the file must include the other file either directly or indirectly.

Include cycles are not allowed among files. If `a.json` includes `b.json`, `b.json` cannot include `a.json`. However, a file may be included multiple times from the same file or from different files.

Files directly or indirectly included from `CMakePresets.json` should be guaranteed to be provided by the project. `CMakeUserPresets.json` may include files from anywhere.

Starting from version `7`, the `include` field supports [macro expansion](preset-macro-expansion), but only `$penv{}` macro expansion.

## References

- https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#id5