# Integer Types

## Character Types



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

###### `signed short int`

**aliases**: `signed short`, `short`
**size**: >= 16 bits.

###### `signed int`

**aliases**: `signed`, `int`
**size**: >= 16 bits. However, on 32/64 bit systems it is almost exclusively guaranteed to have width of at least 32 bits.

###### `signed long int`

**aliases**: `signed long`, `long`
**size**: >= 32 bits

###### `signed long long int`

**aliases**: `signed long long`, `long long`
**size:** >= 64 bits

### Extended Signed Integer Types

**since**: cpp11

The extended integer types are implementation-defined. However standard provides a framework for implementing such extensions in a way that doesn’t interfere with the behavior of standard compliant programs.

###### `intN_t`

Signed integer type with width of exactly `N` bits with no padding bits and using 2's complement for negative values.

**Example**: `int8_t`, `int16_t`, `int32_t`, `int64_t`.

###### `int_fastN_t`

Fastest signed integer type with width of at least `N` bits.

**Example**: `int_fast8_t`, `int_fast16_t`, `int_fast32_t`, `int_fast64_t`

###### `int_leastN_t`

Smallest signed integer type with width of at least `N` bits.

**Example**: `int_least8_t`, `int_least16_t`, `int_least32_t, int_least64_t`
    
###### `intmax_t`

Signed integer type of maximum width.
    
###### `intptr_t`

Signed integer type capable of holding a pointer to `void`.

## Unsigned Integer Types

### Standard Unsigned Integer Types

###### `unsigned short int`

**aliases**: `unsigned short`
**size**: >= 16 bits.

###### `unsigned int`

**aliases**: `unsigned`
**size**: >= 16 bits. However, on 32/64 bit systems it is almost exclusively guaranteed to have width of at least 32 bits.

###### `unsigned long int`

**aliases**: `unsigned long`
**size**: >= 32 bits.

###### `unsigned long long int`

**aliases**: `unsigned long long`
**size**: >= 64 bits.

### Extended Unsigned Integer Types

**since**: cpp11

The extended integer types are implementation-defined. However standard provides a framework for implementing such extensions in a way that doesn’t interfere with the behavior of standard compliant programs.

###### `unitN_t`

Unsigned integer type with width of exactly `N` bits.

**Example**: `uint8_t`, `uint16_t`, `uint32_t`, `uint64_t`

###### `unit_fastN_t`

Fastest unsigned integer type with width of at least `N` bits.

**Example**: `uint_fast8_t`, `uint_fast16_t`, `uint_fast32_t`, `uint_fast64_t`

###### `unit_leastN_t`

Smallest unsigned integer type with width of at least `N` bits.

**Example**: `uint_least8_t`, `uint_least16_t`, `uint_least32_t`, `uint_least64_t`

###### `uintmax_t`

Unsigned integer type of maximum width.

###### `uintptr_t`

Unsigned integer type capable of holding a pointer to void.

## References

- https://en.cppreference.com/w/cpp/language/types

- https://en.cppreference.com/w/cpp/types/integer

- https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2709.pdf

- https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2006/n1988.pdf
