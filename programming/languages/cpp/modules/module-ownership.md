# Module Ownership

In general, if a declaration appears after the module declaration in a module unit, it is _attached to_ that module.

If a declaration of an entity is attached to a named module, that entity can only be defined in that module. All declarations of such an entity must be attached to the same module.

If a declaration is attached to a named module, and it is not exported, the declared name has [module linkage](https://en.cppreference.com/w/cpp/language/storage_duration#module_linkage "cpp/language/storage duration").

If two matching declarations are attached to different modules, and they both declare names with external linkage, the program is ill-formed; no diagnostic is required if neither is reachable from the other. In practice, there are two models:

- In the _weak module ownership_ model, such declarations are considered to declare the same entity.
- In the _strong module ownership_ model, they are considered to declare different entities.

## References

- https://en.cppreference.com/w/cpp/language/modules