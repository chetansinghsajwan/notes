# Move Constructor

A move constructor of class `T` is a non-template [constructor](https://en.cppreference.com/w/cpp/language/initializer_list) whose first parameter is `T&&`, `const T&&`, `volatile T&&`, or `const volatile T&&`, and either there are no other parameters, or the rest of the parameters all have default values.

### Implicitly-declared move constructor

The compiler will declare a move constructor as a implicit `inline public` member of its class with the signature `T::T(T&&)` if all of the following are true:

- There are no user-declared move constructors.

- There are no user-declared [copy constructors](https://en.cppreference.com/w/cpp/language/copy_constructor).

- There are no user-declared [copy assignment operators](https://en.cppreference.com/w/cpp/language/copy_assignment).

- There are no user-declared [move assignment operators](https://en.cppreference.com/w/cpp/language/move_assignment).

- There is no user-declared [destructor](https://en.cppreference.com/w/cpp/language/destructor).

A class can have multiple move constructors, e.g. both `T::T(const T&&)` and `T::T(T&&)`.

The user can force the generation of the implicitly declared move constructor with the keyword `default`.

The implicitly-declared (or defaulted on its first declaration) move constructor has an exception specification as described in [dynamic exception specification](https://en.cppreference.com/w/cpp/language/except_spec) (until C++17)[noexcept specification](https://en.cppreference.com/w/cpp/language/noexcept_spec) (since C++17).

### Deleted implicitly-declared move constructor

The implicitly-declared or defaulted move constructor for class `T` is defined as **deleted** if any of the following is true:

- `T` has non-static data members that cannot be moved (have deleted, inaccessible, or ambiguous move constructors).

- `T` has direct or virtual base class that cannot be moved (has deleted, inaccessible, or ambiguous move constructors).

- `T` has direct or virtual base class or a non-static data member with a deleted or inaccessible destructor.

- `T` is a union-like class and has a variant member with non-trivial move constructor.

### Trivial move constructor

The move constructor for class `T` is trivial if all of the following is true:

- It is not user-provided (meaning, it is implicitly-defined or defaulted).

- `T` has no virtual member functions.

- `T` has no virtual base classes.

- The move constructor selected for every direct base of `T` is trivial.

- The move constructor selected for every non-static class type (or array of class type) member of `T` is trivial.

A trivial move constructor is a constructor that performs the same action as the trivial copy constructor.

[_TriviallyCopyable_](https://en.cppreference.com/w/cpp/named_req/TriviallyCopyable) objects can be copied by copying their object representations manually, e.g. with [std::memmove](https://en.cppreference.com/w/cpp/string/byte/memmove).

### Implicitly-defined move constructor

If the implicitly-declared move constructor is neither deleted nor trivial, it is defined (that is, a function body is generated and compiled) by the compiler if [odr-used](https://en.cppreference.com/w/cpp/language/definition#ODR-use) or [needed for constant evaluation](https://en.cppreference.com/w/cpp/language/constant_expression#Functions_and_variables_needed_for_constant_evaluation).

For union types, the implicitly-defined move constructor copies the object epresentation (as by [std::memmove](https://en.cppreference.com/w/cpp/string/byte/memmove)).

For non-union class types (class and struct), the move constructor performs full member-wise move of the object's bases and non-static members, in their initialization order, using direct initialization with an [xvalue](https://en.cppreference.com/w/cpp/language/value_category#xvalue) argument.
