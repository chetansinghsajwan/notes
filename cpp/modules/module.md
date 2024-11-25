# Module

It is a collection of module-units with the same module-name in their module-declaration.

A user defined module is also called named-module. Only global-module is the only unnamed-module.

For every named-module, there must be exactly one module-interface-unit that specifies no module partition.

This module unit is termed the primary-module-interface-unit. 

Its exported content will be available when importing the corresponding named module.

## References

 - https://en.cppreference.com/w/cpp/language/modules