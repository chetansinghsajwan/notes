# Header Unit

A header unit is a separate translation unit synthesized from a header.

Importing a header-unit will make accessible all its definitions and declarations. Preprocessor macros are also accessible (because import declarations are recognized by the preprocessor).

However, contrary to `#include`, preprocessing macros already defined at the point of the import declaration will not affect the processing of the header.

---

**Example**:

```cpp
// main.cpp
export module main;
 
import <iostream>;
export import <string_view>;
```

---

## References

- https://en.cppreference.com/w/cpp/language/modules