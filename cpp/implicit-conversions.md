# Implicit Conversions

An **implicit conversion sequence** is the sequence of conversions required to convert an expression to the type of the corresponding parameter in a function declaration.

#### Order of the conversions

Implicit conversion sequence consists of the following, in this order:

1. zero or one _standard conversion sequence._

2. zero or one _user-defined conversion._

3. zero or one _standard conversion sequence_ (only if a _user-defined conversion_ is used).

When considering the argument to a constructor or to a user-defined conversion function, only one standard conversion sequence is allowed (otherwise user-defined conversions could be effectively chained).

When converting from one non-class type to another non-class type, only a standard conversion sequence is allowed.

A standard conversion sequence consists of the following, in this order:

1. zero or one conversion from the following set:
    - _lvalue-to-rvalue conversion_,
    - _array-to-pointer conversion_, and
    - _function-to-pointer conversion_;

2. zero or one _numeric promotion_ or _numeric conversion._

3. zero or one _function pointer conversion._ **(since c++17)**

4. zero or one _qualification conversion_.

A user-defined conversion consists of zero or one implicit single-argument [converting constructor](https://en.cppreference.com/w/cpp/language/converting_constructor) or implicit [conversion function](https://en.cppreference.com/w/cpp/language/cast_operator) call.

An expression e is said to be _implicitly convertible to_ `_T2_` if and only if `T2` can be [copy-initialized](https://en.cppreference.com/w/cpp/language/copy_initialization) from e, that is the declaration T2 t = e; is well-formed (can be compiled), for some invented temporary `t`. Note that this is different from [direct initialization](https://en.cppreference.com/w/cpp/language/direct_initialization) (T2 t(e)), where explicit constructors and conversion functions would additionally be considered.