# Floating Point Types

## Standard Floating Point Types

The following three types and their cv-qualified versions are collectively called standard floating-point types.

###### `float` type

Single precision floating-point type.

Matches [IEEE-754 binary32 format](https://en.wikipedia.org/wiki/Single-precision_floating-point_format) if supported.

###### `double` type

Double precision floating-point type.

Matches [IEEE-754 binary64 format](https://en.wikipedia.org/wiki/Double-precision_floating-point_format) if supported.

###### `long double` type

Extended precision floating-point type.

The most well known IEEE-754 binary64-extended format is [x87 80-bit extended precision format](https://en.wikipedia.org/wiki/Extended_precision#x86_extended_precision_format). It is used by many x86 and x86-64 implementations (a notable exception is MSVC, which implements long double in the same format as double, i.e. binary64).

## Extended Floating Point Types

**since**: cpp23

The extended floating-point types are implementation-defined. They may include [fixed width floating-point types](https://en.cppreference.com/w/cpp/types/floating-point).

## Properties

Floating-point types may support [special values](https://en.cppreference.com/w/cpp/types/numeric_limits):

- _infinity_ (positive and negative), see [INFINITY](https://en.cppreference.com/w/cpp/numeric/math/INFINITY)

- the _negative zero_, -0.0. It compares equal to the positive zero, but is meaningful in some arithmetic operations, e.g. 1.0 / 0.0 == [INFINITY](http://en.cppreference.com/w/cpp/numeric/math/INFINITY), but 1.0/-0.0 == -[INFINITY](http://en.cppreference.com/w/cpp/numeric/math/INFINITY)), and for some mathematical functions, e.g. [sqrt(std::complex)](https://en.cppreference.com/w/cpp/numeric/complex/sqrt)

- _not-a-number_ (NaN), which does not compare equal with anything (including itself). Multiple bit patterns represent NaNs, see [std::nan](https://en.cppreference.com/w/cpp/numeric/math/nan), [NAN](https://en.cppreference.com/w/cpp/numeric/math/NAN). Note that C++ takes no special notice of signalling NaNs other than detecting their support by [std::numeric_limits::has_signaling_NaN](https://en.cppreference.com/w/cpp/types/numeric_limits/has_signaling_NaN), and treats all NaNs as quiet.

Real floating-point numbers may be used with [arithmetic operators](https://en.cppreference.com/w/cpp/language/operator_arithmetic) +, -, /, and * as well as various mathematical functions from ["cmath"](https://en.cppreference.com/w/cpp/header/cmath).

Both built-in operators and library functions may raise floating-point exceptions and set [errno](https://en.cppreference.com/w/cpp/error/errno) as described in [math errhandling](https://en.cppreference.com/w/cpp/numeric/math/math_errhandling).
Floating-point expressions may have greater range and precision than indicated by their types, see [FLT_EVAL_METHOD](https://en.cppreference.com/w/cpp/types/climits/FLT_EVAL_METHOD).

Floating-point expressions may also be _contracted_, that is, calculated as if all intermediate values have infinite range and precision, see [#pragma STDC FP_CONTRACT](https://en.cppreference.com/w/cpp/preprocessor/impl#.23pragma_STDC). Standard C++ does not restrict the accuracy of floating-point operations.

Some operations on floating-point numbers are affected by and modify the state of [the floating-point environment](https://en.cppreference.com/w/cpp/numeric/fenv) (most notably, the rounding direction).

[Implicit conversions](https://en.cppreference.com/w/cpp/language/implicit_conversion) are defined between real floating types and integer types.

See [Limits of floating-point types](https://en.cppreference.com/w/cpp/types/climits#Limits_of_floating_point_types) and [std::numeric_limits](https://en.cppreference.com/w/cpp/types/numeric_limits) for additional details, limits, and properties of the floating-point types.

## References

- https://en.cppreference.com/w/cpp/language/types
