# Zero Initialization

Zero-initialization is performed in the following situations:

- For every named variable with static or thread-local(since C++11) [storage duration](https://en.cppreference.com/w/cpp/language/storage_duration "cpp/language/storage duration") that is not subject to [constant initialization](https://en.cppreference.com/w/cpp/language/constant_initialization "cpp/language/constant initialization"), before any other initialization.

- As part of [value-initialization](https://en.cppreference.com/w/cpp/language/value_initialization "cpp/language/value initialization") sequence for non-class types and for members of value-initialized class types that have no constructors, including value initialization of elements of [aggregates](https://en.cppreference.com/w/cpp/language/aggregate_initialization "cpp/language/aggregate initialization") for which no initializers are provided.

- When an array of any [character type](cpp/types/character-types) is [initialized with a string literal](https://en.cppreference.com/w/cpp/language/aggregate_initialization#Character_arrays "cpp/language/aggregate initialization") that is too short, the remainder of the array is zero-initialized.

The effects of zero-initialization are:

- If `T` is a [scalar type](https://en.cppreference.com/w/cpp/named_req/ScalarType "cpp/named req/ScalarType"), the object is initialized to the value obtained by [explicitly converting](https://en.cppreference.com/w/cpp/language/explicit_cast "cpp/language/explicit cast") the integer literal ​0​ (zero) to `T`.

- If `T` is a non-union class type:
    - all [padding bits](https://en.cppreference.com/w/cpp/language/object#Object_representation_and_value_representation "cpp/language/object") are initialized to zero bits,
    - each non-static [data member](https://en.cppreference.com/w/cpp/language/data_members "cpp/language/data members") is zero-initialized,
    - each non-virtual base class [subobject](https://en.cppreference.com/w/cpp/language/object#Subobjects "cpp/language/object") is zero-initialized, and
    - if the object is not a base class subobject, each [virtual base class](https://en.cppreference.com/w/cpp/language/derived_class#Virtual_base_classes "cpp/language/derived class") subobject is zero-initialized.

- If `T` is a union type:
    - all padding bits are initialized to zero bits, and
    - the object’s first non-static named data member is zero-initialized.

- If `T` is array type, each element is zero-initialized.
- If `T` is reference type, nothing is done.

---

**Example:**

```cpp
#include <iostream>
#include <string>
 
struct A
{
    int a, b, c;
};
 
double f[3];   // zero-initialized to three 0.0's
 
int* p;        // zero-initialized to null pointer value
               // (even if the value is not integral 0)
 
[std::string](http://en.cppreference.com/w/cpp/string/basic_string) s; // zero-initialized to indeterminate value, then
               // default-initialized to "" by the std::string default constructor
 
int main(int argc, char*[])
{
    delete p; // safe to delete a null pointer
 
    static int n = argc; // zero-initialized to 0 then copy-initialized to argc
    [std::cout](http://en.cppreference.com/w/cpp/io/cout) << "n = " << n << '\n';
 
    A a = A(); // the effect is same as: A a{}; or A a = {};
    [std::cout](http://en.cppreference.com/w/cpp/io/cout) << "a = {" << a.a << ' ' << a.b << ' ' << a.c << "}\n";
}
```

Output:

```
n = 1
a = {0 0 0}
```

---

## References

- https://en.cppreference.com/w/cpp/language/zero_initialization
