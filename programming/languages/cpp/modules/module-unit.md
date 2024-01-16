# Module Unit

It is a [translation unit]() that contains a [module-declaration]().

**module-interface-unit**: A [module-unit]() whose [module-declaration]() has the keyword `export`.

**module-implementation-unit**: A [module-unit]() which is not a module-interface-unit.

Each module-unit is associated to a module name (and optionally a partition), provided in the module declaration.

#### Module Declaration
^module-declaration

It is a statement which defines a module-unit.

It must be the first declaration of the translation unit (except the [[#^global-module-fragment]]).

---

**Syntax**:

`[export] module <module-name> [module-partition-name] [attr]`

where,

**`export`**: if present, specifies this is a [[#^module-interface-unit]].
**`module-name`**: name of the module.
**`module-partition-name`**: 

---

The **module-name** consists of one or more identifiers separated by dots (for example: `mymodule`, `mymodule.mysubmodule`, `mymodule2`...). Dots have no intrinsic meaning, however they are used informally to represent hierarchy.

#### Global Module Fragement
^global-module-fragement

Module units can be prefixed by a _global module fragment_, which can be used to include headers when importing the headers is not possible.

If a module-unit has a global module fragment, then its first declaration must be `**module;**`. Then, only [preprocessing directives](https://en.cppreference.com/w/cpp/preprocessor#Directives "cpp/preprocessor") can appear in the global module fragment. Then, a standard module declaration marks the end of the global module fragment and the start of the module content.

#### Private Module Fragement
^private-module-fragement

## References

- https://en.cppreference.com/w/cpp/language/modules