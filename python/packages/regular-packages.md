# Regular Packages

Regular packages are traditional packages as they existed in Python 3.2 and earlier.

A regular package is typically implemented as a directory containing an `__init__.py` file. When a regular package is imported, this `__init__.py` file is implicitly executed, and the objects it defines are bound to names in the package’s namespace.

```
parent/
  __init__.py
  one/
	__init__.py
  two/
    __init__.py
  three/
    __init__.py
```

Importing `parent.one` will implicitly execute `parent/__init__.py` and `parent/one/__init__.py`.

## References

- https://docs.python.org/3/reference/import.html#regular-packages
