# Copy Initialization

Initializes an object from another object.

Copy-initialization is performed in the following situations:

- When a named variable (automatic, static, or thread-local) of a non-reference type `T` is declared with the initializer consisting of an equals sign followed by an expression.

- When [passing an argument](https://en.cppreference.com/w/cpp/language/operator_other#Built-in_function_call_operator "cpp/language/operator other") to a function by value.

- When [returning](https://en.cppreference.com/w/cpp/language/return "cpp/language/return") from a function that returns by value.

- When [throwing](https://en.cppreference.com/w/cpp/language/throw "cpp/language/throw") or [catching](https://en.cppreference.com/w/cpp/language/try_catch "cpp/language/try catch") an exception by value.

- As part of [aggregate initialization](aggregate-initialization), to initialize each element for which an initializer is provided.

## Refeences

- https://en.cppreference.com/w/cpp/language/copy_initialization
