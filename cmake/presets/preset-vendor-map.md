# Preset Vendor Map

A map containing vendor-specific information.

CMake does not interpret the contents of this field except to verify that it is a map if it does exist.

However, the keys should be a vendor-specific domain name followed by a `/`-separated path.

For example, the Example IDE 1.0 could use `example.com/ExampleIDE/1.0`. The value of each field can be anything desired by the vendor, though will typically be a map.

## References

- https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html