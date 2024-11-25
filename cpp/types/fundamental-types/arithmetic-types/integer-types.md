# Integer Types

###### `bool` type

## Character Types

### Narrow Character Types

The character types `char`, `signed char`, `unsigned char` and `char8_t` are considered as narrow character types and `char16_t`, `char32_t` and `wchar_t` are considered as wide character types.

###### `signed char` type
^signed-char

Type for signed character representation.

###### `unsigned char` type
^unsigned-char

Type for unsigned character representation.

###### `char` type
^char

Type for character representation which can be most efficiently processed on the target system.

`char` type is not same as `signed char` or `unsigned char`. But it has the same representation as `signed char` or `unsigned char`.

Size of `char` is always `1`.

The signedness of `char` is implementation defined. The defaults for ARM and PowerPC are typically unsigned, the defaults for x86 and x64 are typically signed.

A `char`, a `signed char`, and an `unsigned char` occupy the same amount of storage and have the same alignment requirements.

### Wide Character Types

###### `wchar_t` type
^wchar

Type for wide character representation (see [wide strings](https://en.cppreference.com/w/cpp/string/wide)). It has the same size, signedness, and alignment as one of the integer types, but is a distinct type.

**Size:**

`sizeof(wchar_t)` is implementation defined. It is generally 16 bits in Windows and 32 bits in other platforms.

**Encoding:**

Character encoding represented by `whar_t` is also implementation defined. It generally represents UTF-16 in windows and UTF-32 encoding in other platforms.

**Standard Library Support:**

The C++ Standard Library provides various functions and templates for working with wide characters and strings, such as `std::wcin`, `std::wcout`, and functions like `std::wstrlen()` and `std::wprintf()`.

###### `char8_t` type
^char8

**since:** cpp20

Type for UTF-8 character representation, required to be large enough to represent any UTF-8 code unit (8 bits).

It has the same size, signedness, and alignment as `unsigned char`, but is a distinct type.

###### `char16_t` type
^char16

**since**: cpp11

Type for utf16 character representation, required to be large enough to represent any UTF-16 code unit (16 bits).

It has the same size, signedness, and alignment as [`uint_least16_t`](https://en.cppreference.com/w/cpp/types/integer), but is a distinct type.

###### `char32_t` type
^char32

**since**: cpp11

Type for utf32 character representation, required to be large enough to represent any UTF-32 code unit (32 bits).

It has the same size, signedness, and alignment as [`uint_least32_t`](https://en.cppreference.com/w/cpp/types/integer), but is a distinct type.

## Signed Integer Types

### Standard Signed Integer Types

There are 8 standard integer types. These types are formed by modifying the basic integer type `int`.

**Signedness modifiers**

`signed`: target type will have signed representation (this is the default if omitted)

`unsigned`: target type will have unsigned representation

**Size modifiers**

`short`: target type will be optimized for space and will have width of at least 16 bits.

`long`: target type will have width of at least 32 bits.

`long long`: target type will have width of at least 64 bits.

###### `signed short int` type

**aliases**: `signed short`, `short`
**size**: >= 16 bits.

###### `signed int` type

**aliases**: `signed`, `int`
**size**: >= 16 bits. However, on 32/64 bit systems it is almost exclusively guaranteed to have width of at least 32 bits.

###### `signed long int` type

**aliases**: `signed long`, `long`
**size**: >= 32 bits

###### `signed long long int` type

**aliases**: `signed long long`, `long long`
**size:** >= 64 bits

### Extended Signed Integer Types
^extendend-signed-integer-types

**since**: cpp11

The extended integer types are implementation-defined. However standard provides a framework for implementing such extensions in a way that doesn’t interfere with the behavior of standard compliant programs.

###### `intN_t` type

Signed integer type with width of exactly `N` bits with no padding bits and using 2's complement for negative values.

**Example**: `int8_t`, `int16_t`, `int32_t`, `int64_t`.

###### `int_fastN_t` type

Fastest signed integer type with width of at least `N` bits.

**Example**: `int_fast8_t`, `int_fast16_t`, `int_fast32_t`, `int_fast64_t`

###### `int_leastN_t` type

Smallest signed integer type with width of at least `N` bits.

**Example**: `int_least8_t`, `int_least16_t`, `int_least32_t, int_least64_t`
    
###### `intmax_t` type

Signed integer type of maximum width.
    
###### `intptr_t` type

Signed integer type capable of holding a pointer to `void`.

## Unsigned Integer Types

### Standard Unsigned Integer Types
^standard-unsigned-integer-types

###### `unsigned short int` type

**aliases**: `unsigned short`
**size**: >= 16 bits.

###### `unsigned int` type

**aliases**: `unsigned`
**size**: >= 16 bits. However, on 32/64 bit systems it is almost exclusively guaranteed to have width of at least 32 bits.

###### `unsigned long int` type

**aliases**: `unsigned long`
**size**: >= 32 bits.

###### `unsigned long long int` type

**aliases**: `unsigned long long`
**size**: >= 64 bits.

### Extended Unsigned Integer Types
^extendend-unsigned-integer-types

**since**: cpp11

The extended integer types are implementation-defined. However standard provides a framework for implementing such extensions in a way that doesn’t interfere with the behavior of standard compliant programs.

###### `unitN_t` type

Unsigned integer type with width of exactly `N` bits.

**Example**: `uint8_t`, `uint16_t`, `uint32_t`, `uint64_t`

###### `unit_fastN_t` type

Fastest unsigned integer type with width of at least `N` bits.

**Example**: `uint_fast8_t`, `uint_fast16_t`, `uint_fast32_t`, `uint_fast64_t`

###### `unit_leastN_t` type

Smallest unsigned integer type with width of at least `N` bits.

**Example**: `uint_least8_t`, `uint_least16_t`, `uint_least32_t`, `uint_least64_t`

###### `uintmax_t` type

Unsigned integer type of maximum width.

###### `uintptr_t` type

Unsigned integer type capable of holding a pointer to void.

## References

- https://en.cppreference.com/w/cpp/language/types

- https://en.cppreference.com/w/cpp/types/integer

- https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2709.pdf

- https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2006/n1988.pdf

- https://twiserandom.com/cpp/cpp-character-types-char-wchar_t-char8_t-char16_t-and-char32_t-a-tutorial/index.html
