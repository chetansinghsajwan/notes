# `std::nullptr_t` type

It is the type of the `nullptr` literal.

It is a distinct type that is not itself a pointer type or a pointer to member type. But its values can be [implicitly converted](https://en.cppreference.com/w/cpp/language/implicit_cast) to any pointer and pointer to member type.

Its size is equal to `void*`.

## References

- https://en.cppreference.com/w/cpp/language/types