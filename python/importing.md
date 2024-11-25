# Importing

Modules can be imported in three ways:
1. [`import` statement](python/statements/import)
2. [`importlib.import_module()` function](python/libs/importlib/functions/import_module)
3. [`__import__()` function](python/functions/__import__)

Python search for module in the following way.

**`sys.modules`**

The first place checked during import search is [`sys.modules`](https://docs.python.org/3/library/sys.html#sys.modules "sys.modules"). This mapping serves as a cache of all modules that have been previously imported, including the intermediate paths. Intermediate paths like `a` and `b` in `a.b.c` module.

Each key will have as its value as the corresponding module object. If the name is present, the associated value is the module satisfying the import, and the process completes.

However, if the value is `None`, then a [`ModuleNotFoundError`](python/errors/module-not-found) is raised. If the module name is missing, Python will continue searching for the module.

**Finders and Loaders**

In this stage python’s import protocol is invoked to find and load the module. This protocol consists of two conceptual objects, [finders](https://docs.python.org/3/glossary.html#term-finder) and [loaders](https://docs.python.org/3/glossary.html#term-loader). A finder’s job is to determine whether it can find the named module using whatever strategy it knows about. Objects that implement both of these interfaces are referred to as [importers](https://docs.python.org/3/glossary.html#term-importer).

If finders find the named module, they return a _module spec_, an encapsulation of the module’s import-related information, which the import machinery then uses when loading the module.

The module will exist in [`sys.modules`](https://docs.python.org/3/library/sys.html#sys.modules "sys.modules") before the loader executes the module code. This is crucial because the module code may (directly or indirectly) import itself; adding it to [`sys.modules`](https://docs.python.org/3/library/sys.html#sys.modules "sys.modules") beforehand prevents unbounded recursion in the worst case and multiple loading in the best.

If loading fails, the failing module – and only the failing module – gets removed from [`sys.modules`](https://docs.python.org/3/library/sys.html#sys.modules "sys.modules"). Any module already in the [`sys.modules`](https://docs.python.org/3/library/sys.html#sys.modules "sys.modules") cache, and any module that was successfully loaded as a side-effect, must remain in the cache. This contrasts with reloading where even the failing module is left in [`sys.modules`](https://docs.python.org/3/library/sys.html#sys.modules "sys.modules").

> [!info]
> Changed in version 3.4: In previous versions of Python, finders returned [loaders](https://docs.python.org/3/glossary.html#term-loader) directly, whereas now they return module specs which _contain_ loaders. Loaders are still used during import but have fewer responsibilities.

Python includes a number of default finders and importers. The first one knows how to locate built-in modules, and the second knows how to locate frozen modules. A third default finder searches an [import path](https://docs.python.org/3/glossary.html#term-import-path) for modules. The [import path](https://docs.python.org/3/glossary.html#term-import-path) is a list of locations that may name file system paths or zip files. It can also be extended to search for any locatable resource, such as those identified by URLs.

New finders can be added to extend the range and scope of module searching.

## References

- https://docs.python.org/3/reference/import.html
