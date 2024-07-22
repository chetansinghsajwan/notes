# Modules

Modules are a language feature to share declarations and definitions across translation units.

They were added in cpp20.

#### Exporting declarations and definitions

Module interface units can export declarations (including definitions), which can be imported by other translation units. To export a declaration, either prefix it with the `**export**` keyword, or else place it inside an `**export**` block.

#### Importing modules and header units

Modules are imported via an _import declaration_:

All declarations and definitions exported in the module interface units of the given named module will be available in the translation unit using the import declaration.

Import declarations can be exported in a module interface unit. That is, if module B export-imports A, then importing B will also make visible all exports from A.

In module units, all import declarations (including export-imports) must be grouped after the module declaration and before all other declarations.

## References

- https://en.cppreference.com/w/cpp/language/modules