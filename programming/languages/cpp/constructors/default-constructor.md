# Default Constructor

A default constructor is a [constructor](https://en.cppreference.com/w/cpp/language/constructor) which can be called with no arguments (either defined with an empty parameter list, or with default arguments provided for every parameter).

A type with a public default constructor is [_DefaultConstructible_](https://en.cppreference.com/w/cpp/named_req/DefaultConstructible).

### Implicitly-declared default constructor

If no user-declared constructors of any kind are provided for a class type (struct, class, or union), the compiler will always declare a default constructor as an `inline public` member of its class.

The user can force the automatic generation of a default constructor by the compiler with the keyword default.