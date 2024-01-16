# Module Partition

A module can have _module partition units_. They are module units whose module declarations include a module partition, which starts with a colon `**:**` and is placed after the module name.

A module partition represents exactly one module unit (two module units cannot designate the same module partition). They are visible only from inside the named module (translation units outside the named module cannot import a module partition directly).

A module partition can be imported by module units of the same named module.

## References

- https://en.cppreference.com/w/cpp/language/modules
