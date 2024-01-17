# Module Partition

It is a way to divide modules into partitions, which gives them more control and flexibility.

Its declaration is module-name, colon and module-partition-name.

```cpp
export module my_module:my_module_partition;
```

Each module-partition contains only one unit (two module units cannot designate the same module partition).

Each module-partition is visible only from inside the module (translation units outside the named module cannot import a module partition directly).

A module partition can be imported by module-units of the same module.

---

**Example**:

```cpp
/////// A-B.cpp   
export module A:B;
...

/////// A-C.cpp
module A:C;
...

/////// A.cpp
export module A;

import :C;
export import :B;
```

---

## References

- https://en.cppreference.com/w/cpp/language/modules
