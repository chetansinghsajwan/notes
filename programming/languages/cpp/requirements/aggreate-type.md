# Aggregate Type

It is one of the following types:

- [array type](cpp/types/array-type)
- [class type](cpp/types/class-type) that has
    - no user-declared or inherited constructors
    - no private or protected direct(since C++17) non-static data members
    - no [virtual base classes](https://en.cppreference.com/w/cpp/language/derived_class#Virtual_base_classes "cpp/language/derived class")
    - no [private](https://en.cppreference.com/w/cpp/language/derived_class#Private_inheritance "cpp/language/derived class") or [protected](https://en.cppreference.com/w/cpp/language/derived_class#Protected_inheritance "cpp/language/derived class") direct base classes
    - no virtual member functions

## References

- https://en.cppreference.com/w/cpp/language/aggregate_initialization