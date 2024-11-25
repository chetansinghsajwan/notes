# Modules

## Importing

Modules can be imported in three ways:
1. [`import` statement](python/statements/import)
2. [`importlib.import_module()` function](python/libs/importlib/functions/import_module)
3. `__import__()` function

A direct call to [`__import__()`](https://docs.python.org/3/library/functions.html#import__ "__import__") performs only the module search and, if found, the module creation operation, only the [`import`](https://docs.python.org/3/reference/simple_stmts.html#import) statement performs a name binding operation.

When an [`import`](https://docs.python.org/3/reference/simple_stmts.html#import) statement is executed, the standard builtin [`__import__()`](https://docs.python.org/3/library/functions.html#import__ "__import__") function is called. Other mechanisms for invoking the import system (such as [`importlib.import_module()`](https://docs.python.org/3/library/importlib.html#importlib.import_module "importlib.import_module")) may choose to bypass [`__import__()`](https://docs.python.org/3/library/functions.html#import__ "__import__") and use their own solutions to implement import semantics.

See [`importlib`](python/modules/importlib)

Python has only one type of module object, and all modules are of this type, regardless of whether the module is implemented in Python, C, or something else.

## Module Searching

**`sys.modules`**

The first place checked during import search is [`sys.modules`](https://docs.python.org/3/library/sys.html#sys.modules "sys.modules"). This mapping serves as a cache of all modules that have been previously imported, including the intermediate paths.

Each key will have as its value the corresponding module object. If the name is present, the associated value is the module satisfying the import, and the process completes.

However, if the value is `None`, then a [`ModuleNotFoundError`](python/errors/module-not-found) is raised. If the module name is missing, Python will continue searching for the module.

**Finders and Loaders**

If the named module is not found in [`sys.modules`](https://docs.python.org/3/library/sys.html#sys.modules "sys.modules"), then Python’s import protocol is invoked to find and load the module. This protocol consists of two conceptual objects, [finders](https://docs.python.org/3/glossary.html#term-finder) and [loaders](https://docs.python.org/3/glossary.html#term-loader). A finder’s job is to determine whether it can find the named module using whatever strategy it knows about. Objects that implement both of these interfaces are referred to as [importers](https://docs.python.org/3/glossary.html#term-importer) - they return themselves when they find that they can load the requested module.

Python includes a number of default finders and importers. The first one knows how to locate built-in modules, and the second knows how to locate frozen modules. A third default finder searches an [import path](https://docs.python.org/3/glossary.html#term-import-path) for modules. The [import path](https://docs.python.org/3/glossary.html#term-import-path) is a list of locations that may name file system paths or zip files. It can also be extended to search for any locatable resource, such as those identified by URLs.

New finders can be added to extend the range and scope of module searching.

If finders find the named module, they return a _module spec_, an encapsulation of the module’s import-related information, which the import machinery then uses when loading the module.

> [!info]
> Changed in version 3.4: In previous versions of Python, finders returned [loaders](https://docs.python.org/3/glossary.html#term-loader) directly, whereas now they return module specs which _contain_ loaders. Loaders are still used during import but have fewer responsibilities.

## References

- https://docs.python.org/3/reference/import.html
	