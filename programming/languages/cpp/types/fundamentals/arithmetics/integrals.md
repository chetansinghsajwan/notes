# Integer Types

## **Standard integer types**
`int`: basic integer type.
- The keyword int may be omitted if any of the modifiers listed below are used.
- If no length modifiers are present, it's guaranteed to have a width of at least 16 bits. However, on 32/64 bit systems it is almost exclusively guaranteed to have width of at least 32 bits.
**Modifiers**
Modifies the basic integer type. Can be mixed in any order. Only one of each group can be present in type name.
**Signedness modifiers**
`signed`: target type will have signed representation (this is the default if omitted)
`unsigned`: target type will have unsigned representation
**Size modifiers**
`short`: target type will be optimized for space and will have width of at least 16 bits.
`long`: target type will have width of at least 32 bits.
`long long`: target type will have width of at least 64 bits.
Note: as with all type specifiers, any order is permitted.
`unsigned long long int` and `long int unsigned long` name the same type.
### **Other integer types**
`size_t`: unsigned integer type of the result of the [sizeof](https://en.cppreference.com/w/cpp/language/sizeof) operator as well as the [sizeof...](https://en.cppreference.com/w/cpp/language/sizeof...) operator and the [alignof](https://en.cppreference.com/w/cpp/language/alignof) operator (since C++11).
- The bit width of `std::size_t` is not less than 16.
`ptrdiff_t`: Type for the difference of two pointers.
### Fixed width integer types
- `**int8_t**`**,** `**int16_t**`**,** `**int32_t**`**,** `**int64_t**`(optional)
    
    Signed integer type with width of exactly 8, 16, 32 and 64 bits respectively with no padding bits and using 2's complement for negative values(provided if and only if the implementation directly supports the type)
    
- `**int_fast8_t**`**,** `**int_fast16_t**`**,** `**int_fast32_t**`**,** `**int_fast64_t**`
    
    Fastest signed integer type with width of at least 8, 16, 32 and 64 bits respectively.
    
- `**int_least8_t**`**,** `**int_least16_t**`**,** `**int_least32_t, int_least64_t**`
    
    Smallest signed integer type with width of at least 8, 16, 32 and 64 bits respectively
    
- `**intmax_t**`
    
    Maximum-width signed integer type
    
- `**intptr_t**`(optional)
    
    Signed integer type capable of holding a pointer to void.
    
- `**uint8_t**`**,** `**uint16_t**`**,** `**uint32_t**`**,** `**uint64_t**`(optional)
    
    Unsigned integer type with width of exactly 8, 16, 32 and 64 bits respectively(provided if and only if the implementation directly supports the type)
    
- `**uint_fast8_t**`**,** `**uint_fast16_t**`**,** `**uint_fast32_t**`**,** `**uint_fast64_t**`
    
    Fastest unsigned integer type with width of at least 8, 16, 32 and 64 bits respectively
    
- `**uint_least8_t**`**,** `**uint_least16_t**`**,** `**uint_least32_t**`**,** `**uint_least64_t**`
    
    Smallest unsigned integer type with width of at least 8, 16, 32 and 64 bits respectively
    
- `**uintmax_t**`
    
    Maximum-width unsigned integer type
    
- `**uintptr_t**`(optional)
    
    Unsigned integer type capable of holding a pointer to void
    
### **Extended integer types (since C++11)**
The extended integer types are implementation-defined.
# References

> [!info]  
>  
> [https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2709.pdf](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2709.pdf)  

> [!info]  
>  
> [https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2006/n1988.pdf](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2006/n1988.pdf)  

> [!info] What are fixed-width integers?  
> Fixed-width integers are integral types with a fixed number of bits.  
> [https://64.github.io/cpp-faq/fixed-width-integers/](https://64.github.io/cpp-faq/fixed-width-integers/)