# Modules

- since: cpp20

- Modules eliminate or reduce many of the problems associated with the use of header files.
- You can import modules in any order without concern for macro redefinitions.
- After a module is compiled once, the results are stored in a binary file that describes all the exported types, functions, and templates. The compiler can process that file much faster than a header file. And, the compiler can reuse it every place where the module is imported in a project.
- `export module module-name;` `module module-name;` The module name consists of one or more identifiers separated by dots (for example: `mymodule`, `mymodule.mysubmodule`, `mymodule2`). Dots have no intrinsic meaning, however they are used informally to represent hierarchy.
- A _module unit_ is a _translation unit_ that has a module declaration.
- Module units whose declaration has the keyword `**export**` are termed _module interface units_; all other module units are termed _module implementation units_.
  
- A module-declaration is a statement near the top of a translation unit that specifies the module name to which this module unit belongs.
- A module-unit is a translation unit that has a module declaration.
- A module-interface-unit is a module unit that has `export` keyword in their module declaration.
- A module-implementation-unit is a module-unit that is not a module-interface-unit.
- A named-module is the collection of module units with the same module name.
- Every user defined module is a named-module.
- Only global-module is the only unnamed-module;
- For every named-module, there must be exactly one module-interface-unit that specifies no module partition; this module unit is termed the primary-module-interface-unit. Its exported content will be available when importing the corresponding named module.
```
// Module inteface unit.
export module A;  // Module declaration.
```
```
// Module implementation unit.
module A;  // Module declaration.
```
```
// Not a module unit.
import A;
int main() { }
```
```
// (each line represents a separate translation unit)
export module A;   // declares the primary module interface unit for named module 'A'
module A;          // declares a module implementation unit for named module 'A'
module A;          // declares another module implementation unit for named module 'A'
export module A.B; // declares the primary module interface unit for named module 'A.B'
module A.B;        // declares a module implementation unit for named module 'A.B'
```
### **Exporting declarations and definitions**
- Module interface units can export declarations (including definitions), which can be imported by other translation units.
- To export a declaration, either prefix it with the `**export**` keyword, or else place it inside an `**export**` block.
- `**export**` `_declaration_`
- `**export {**` `_declaration-seq_` ﻿`(optional) }` 
```
// declares the primary module interface unit for named module 'A'
export module A;
// hello() will be visible by translations units importing 'A'
export char const* hello() { return "hello"; }
// world() will NOT be visible.
char const* world() { return "world"; }
// Both one() and zero() will be visible.
export
{
    int one()  { return 1; }
    int zero() { return 0; }
}
// Exporting namespaces also works: hi::english() and hi::french() will be visible.
export namespace hi
{
    char const* english() { return "Hi!"; }
    char const* french()  { return "Salut!"; }
}
```
# **Importing modules and headers**
- `export`==`(optional)`== `import module-name;`
- All declarations and definitions exported in the module interface units of the given named module will be available only in the translation unit using the import declaration.
- Import declarations can be exported in a module interface unit. That is, if module A `export-imports` B, then importing A will also make visible all exports from B.
- In module units, all import declarations (including `export-imports`) must be grouped after the module declaration and before all other declarations.
- `\#include` should not be used in a module unit (outside the _global module fragment_), because all included declarations and definitions would be considered part of the module. Instead, headers can also be imported with an _import declaration_:
- Importing a header will make accessible all its definitions and declarations. Preprocessor macros are also accessible (because import declarations are recognized by the preprocessor). However, contrary to `\#include`, preprocessing macros defined in the translation unit will not affect the processing of the header. This may be inconvenient in some cases (some headers use preprocessing macros as a form of configuration), in which case the usage of _global module fragment_ is needed.
```
/////// A.cpp (primary module interface unit of 'A')
export module A;
export char const* hello() { return "hello"; }
/////// B.cpp (primary module interface unit of 'B')
export module B;
export import A;
export char const* world() { return "world"; }
/////// main.cpp (not a module unit)
\#include <iostream>
import B;
int main()
{
    std::cout << hello() << ' ' << world() << '\n';
}
```
# **Global module fragment**
- Module units can be prefixed by a _global module fragment_, which can be used to include headers when importing the headers is not possible (notably when the header uses preprocessing macros as configuration).
- If a module-unit has a global module fragment, then its first declaration must be `**module;**`. Then, only [preprocessing directives](https://en.cppreference.com/w/cpp/preprocessor#Directives) can appear in the global module fragment. Then, a standard module declaration marks the end of the global module fragment and the start of the module content.
```
/////// A.cpp (primary module interface unit of 'A')
module;
// Defining _POSIX_C_SOURCE adds functions to standard headers,
// according to the POSIX standard.
\#define _POSIX_C_SOURCE 200809L
\#include <stdlib.h>
export module A;
import <ctime>;
// Only for demonstration (bad source of randomness).
// Use C++ <random> instead.
export double weak_random()
{
    std::timespec ts;
    std::timespec_get(&ts,TIME_UTC); // from <ctime>
    // Provided in <stdlib.h> according to the POSIX standard.
    srand48(ts.tv_nsec);
    // drand48() returns a random number between 0 and 1.
    return drand48();
}
/////// main.cpp (not a module unit)
import <iostream>;
import A;
int main()
{
    std::cout << "Random value between 0 and 1: " << weak_random() << '\n';
}
```
# **Private module fragment**
- _Private module fragment_ ends the portion of the module interface unit that can affect the behavior of other translation units.
- If a module unit contains a _private module fragment_, it will be the only module unit of its module.
```
export module foo;
 
export int f();
 
module : private; // ends the portion of the module interface unit that
                  // can affect the behavior of other translation units
                  // starts a private module fragment
 
int f()           // definition not reachable from importers of foo
{
    return 42;
}
```
# Module Partitions
- A module can have _module partition units_.
- A module partition cannot have nested module partition units.
They are module units whose module declarations include a module partition, which starts with a colon `**:**` and is placed after the module name.
```
// Declares a module interface unit for module 'A', partition ':B'.
export module A:B;
```
# References

> [!info] https://omnigoat.github.io/2020/01/19/cpp20-concepts/  
>  
> [https://omnigoat.github.io/2020/01/19/cpp20-concepts/](https://omnigoat.github.io/2020/01/19/cpp20-concepts/)