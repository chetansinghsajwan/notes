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

The module-name consists of one or more identifiers separated by dots (for example: `mymodule`, `mymodule.mysubmodule`, `mymodule2`...). Dots have no intrinsic meaning, however they are used informally to represent hierarchy.

#### Global Module Fragement
^global-module-fragement

It is an optional section at the top of module unit, which can be used to include headers when importing the headers is not possible.

It is declared before module-declaration using `module;` statement.

A standard module-declaration marks the end of the global module fragment and the start of the module content.

---

**Example**:

```cpp
module;
// global-module-fragement starts here

#define SOME_MACRO 0
#include <stdlib.h>

// global-module-fragement ends here
export module my_module;
import std.core;

// ... 
```

---

#### Private Module Fragement
^private-module-fragement

## References

- https://en.cppreference.com/w/cpp/language/modules