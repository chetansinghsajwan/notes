# Package

To help organize modules and provide a naming hierarchy, Python has a concept of [packages](https://docs.python.org/3/glossary.html#term-package).

Like file system directories, packages are organized hierarchically, and packages may themselves contain subpackages, as well as regular modules.

It’s important to keep in mind that all packages are modules, but not all modules are packages. Or put another way, packages are just a special kind of module. Specifically, any module that contains a `__path__` attribute is considered a package.

Python defines two types of packages, [regular packages](regular-package.md) and [namespace packages](namespace-package.md)

## References

- https://docs.python.org/3/reference/import.html#packages
