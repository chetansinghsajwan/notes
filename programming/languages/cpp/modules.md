# Modules

- since: cpp20

They are orthogonal to namespaces, which means that they are independent of namespaces.

**module-unit**: A translation unit that contains a module-declaration.

**module-interface-unit**: A module-unit whose declaration has the keyword **`export`**.

**module-implementation-unit**: A module-unit which is not a module-interface-unit.

**module-declaration**: A statement which defines a module-unit.

**named module**: A collection of module-units with the same module name in their module-declaration.

**header-unit**: A header unit is a separate translation unit synthesized from a header. Importing a header unit will make accessible all its definitions and declarations. Preprocessor macros are also accessible (because import declarations are recognized by the preprocessor).

However, contrary to #include, preprocessing macros already defined at the point of the import declaration will not affect the processing of the header.

## Module Declarations

The module declaration must be the first declaration of the translation unit (excepted the _global module fragment_, which is covered later on).

Each module unit is associated to a module name (and optionally a partition), provided in the module declaration.

###### Syntax

`export`(optional) `module` module-name module-partition ﻿(optional) attr ﻿(optional) `;`

The **module-name** consists of one or more identifiers separated by dots (for example: `mymodule`, `mymodule.mysubmodule`, `mymodule2`...). Dots have no intrinsic meaning, however they are used informally to represent hierarchy.

## Named Module

For every named module, there must be exactly one module interface unit that specifies no module partition; this module unit is termed the _primary module interface unit_. Its exported content will be available when importing the corresponding named module.

### Exporting declarations and definitions

Module interface units can export declarations (including definitions), which can be imported by other translation units. To export a declaration, either prefix it with the `**export**` keyword, or else place it inside an `**export**` block.

### Importing modules and header units

Modules are imported via an _import declaration_:

All declarations and definitions exported in the module interface units of the given named module will be available in the translation unit using the import declaration.

Import declarations can be exported in a module interface unit. That is, if module B export-imports A, then importing B will also make visible all exports from A.

In module units, all import declarations (including export-imports) must be grouped after the module declaration and before all other declarations.

### Global module fragment

Module units can be prefixed by a _global module fragment_, which can be used to include headers when importing the headers is not possible.

If a module-unit has a global module fragment, then its first declaration must be `**module;**`. Then, only [preprocessing directives](https://en.cppreference.com/w/cpp/preprocessor#Directives "cpp/preprocessor") can appear in the global module fragment. Then, a standard module declaration marks the end of the global module fragment and the start of the module content.

### Module partitions

A module can have _module partition units_. They are module units whose module declarations include a module partition, which starts with a colon `**:**` and is placed after the module name.

A module partition represents exactly one module unit (two module units cannot designate the same module partition). They are visible only from inside the named module (translation units outside the named module cannot import a module partition directly).

A module partition can be imported by module units of the same named module.

### Module Ownership

In general, if a declaration appears after the module declaration in a module unit, it is _attached to_ that module.

If a declaration of an entity is attached to a named module, that entity can only be defined in that module. All declarations of such an entity must be attached to the same module.

If a declaration is attached to a named module, and it is not exported, the declared name has [module linkage](https://en.cppreference.com/w/cpp/language/storage_duration#module_linkage "cpp/language/storage duration").

If two matching declarations are attached to different modules, and they both declare names with external linkage, the program is ill-formed; no diagnostic is required if neither is reachable from the other. In practice, there are two models:

- In the _weak module ownership_ model, such declarations are considered to declare the same entity.
- In the _strong module ownership_ model, they are considered to declare different entities.
## References

- https://en.cppreference.com/w/cpp/language/modules