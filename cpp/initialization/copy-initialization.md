# Copy Initialization

Initializes an object from another object.

Copy-initialization is performed in the following situations:

- When a named variable (automatic, static, or thread-local) of a non-reference type `T` is declared with the initializer consisting of an equals sign followed by an expression.

- When [passing an argument](https://en.cppreference.com/w/cpp/language/operator_other#Built-in_function_call_operator "cpp/language/operator other") to a function by value.

- When [returning](https://en.cppreference.com/w/cpp/language/return "cpp/language/return") from a function that returns by value.

- When [throwing](https://en.cppreference.com/w/cpp/language/throw "cpp/language/throw") or [catching](https://en.cppreference.com/w/cpp/language/try_catch "cpp/language/try catch") an exception by value.

- As part of [aggregate initialization](aggregate-initialization.md), to initialize each element for which an initializer is provided.

The effects of copy-initialization are:

- Since cpp17, First, if `T` is a class type and the initializer is a [prvalue](https://en.cppreference.com/w/cpp/language/value_category "cpp/language/value category") expression whose cv-unqualified type is the same class as `T`, the initializer expression itself, rather than a temporary materialized from it, is used to initialize the destination object: see [copy elision](https://en.cppreference.com/w/cpp/language/copy_elision "cpp/language/copy elision").

- Otherwise, if `T` is a class type and the cv-unqualified version of the type of other is `T` or a class derived from `T`, the [non-explicit constructors](https://en.cppreference.com/w/cpp/language/converting_constructor "cpp/language/converting constructor") of `T` are examined and the best match is selected by overload resolution. That constructor is then called to initialize the object.

- Otherwise, if `T` is a class type, and the cv-unqualified version of the type of other is not `T` or derived from `T`, or if `T` is non-class type, but the type of other is a class type, [user-defined conversion sequences](https://en.cppreference.com/w/cpp/language/implicit_cast "cpp/language/implicit cast") that can convert from the type of other to `T` (or to a type derived from `T` if `T` is a class type and a conversion function is available) are examined and the best one is selected through overload resolution. The result of the conversion, which is a rvalue temporary(until C++11)prvalue temporary(since C++11)(until C++17)prvalue expression(since C++17) of the cv-unqualified version of `T` if a [converting constructor](https://en.cppreference.com/w/cpp/language/converting_constructor "cpp/language/converting constructor") was used, is then used to [direct-initialize](https://en.cppreference.com/w/cpp/language/direct_initialization "cpp/language/direct initialization") the object. The last step is usually [optimized out](https://en.cppreference.com/w/cpp/language/copy_elision "cpp/language/copy elision") and the result of the conversion is constructed directly in the memory allocated for the target object, but the appropriate constructor (move or copy) is required to be accessible even though it's not used.(until C++17)

- Otherwise (if neither `T` nor the type of other are class types), [standard conversions](https://en.cppreference.com/w/cpp/language/implicit_cast "cpp/language/implicit cast") are used, if necessary, to convert the value of other to the cv-unqualified version of `T`.

## Refeences

- https://en.cppreference.com/w/cpp/language/copy_initialization
