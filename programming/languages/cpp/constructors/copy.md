# Copy Constructor

A copy constructor of class `T` is a non-template [constructor](https://en.cppreference.com/w/cpp/language/initializer_list) whose first parameter is `T&`, `const T&`, `volatile T&` or `const volatile T&`, and either there are no other parameters, or the rest of the parameters all have default values.

### Implicitly-declared copy constructor

If no user-defined copy constructors are provided for a class type, the compiler will always declare a copy constructor as a `implicit inline public` member of its class.

This implicitly-declared copy constructor has the form `T::T(const T&)` if all of the following are true:

- Each direct and virtual base `B` of `T` has a copy constructor whose parameters are `const B&` or `const volatile B&`.

- Each non-static data member `M` of `T` of class type or array of class type has a copy constructor whose parameters are `const M&` or `const volatile M&`.

Otherwise, the implicitly-declared copy constructor is `T::T(T&)`.

> [!important]  
> 
> Note that due to these rules, the implicitly-declared copy constructor cannot bind to a volatile lvalue argument.  

### Deleted implicitly-declared copy constructor

The **implicitly-declared** or **defaulted** copy constructor for class `T` is defined as **deleted** if any of the following conditions are true:

- `T` has non-static data members that cannot be copied (have deleted, inaccessible, or ambiguous copy constructors).

- `T` has direct or virtual base class that cannot be copied (has deleted, inaccessible, or ambiguous copy constructors).

- `T` has direct or virtual base class or a non-static data member with a deleted or inaccessible destructor.

- `T` is a union-like class and has a variant member with non-trivial copy constructor.

- `T` has a data member of rvalue reference type.

- `T` has a user-defined [move constructor](https://en.cppreference.com/w/cpp/language/move_constructor) or [move assignment operator](https://en.cppreference.com/w/cpp/language/move_assignment) (this condition only causes the implicitly-declared, not the defaulted, copy constructor to be deleted).

### Trivial copy constructor

The copy constructor for class `T` is trivial if all of the following are true:

- It is not user-provided (that is, it is implicitly-defined or defaulted).

- `T` has no virtual member functions.

- `T` has no virtual base classes.

- The copy constructor selected for every direct base of `T` is trivial.

- The copy constructor selected for every non-static class type (or array of class type) member of `T` is trivial.

A trivial copy constructor for a non-union class effectively copies every scalar subobject (including, recursively, subobject of subobjects and so forth) of the argument and performs no other action. However, padding bytes need not be copied, and even the object representations of  
the copied subobjects need not be the same as long as their values are identical.

[_TriviallyCopyable_](https://en.cppreference.com/w/cpp/named_req/TriviallyCopyable) objects can be copied by copying their object representations manually, e.g. with [std::memmove](https://en.cppreference.com/w/cpp/string/byte/memmove).

### Implicitly-defined copy constructor

If the implicitly-declared copy constructor is not deleted, it is defined (that is, a function body is generated and compiled) by the compiler if [odr-used](https://en.cppreference.com/w/cpp/language/definition#ODR-use) or [needed for constant evaluation](https://en.cppreference.com/w/cpp/language/constant_expression#Functions_and_variables_needed_for_constant_evaluation) (since C++11).

For union types, the implicitly-defined copy constructor copies the object representation (as by [std::memmove](https://en.cppreference.com/w/cpp/string/byte/memmove)).

For non-union class types (class and struct), the constructor performs full member-wise copy of the object's bases and non-static members, in their initialization order, using direct initialization.

## Related

- https://en.cppreference.com/w/cpp/language/copy_elision

## References

- https://en.cppreference.com/w/cpp/language/copy_constructor