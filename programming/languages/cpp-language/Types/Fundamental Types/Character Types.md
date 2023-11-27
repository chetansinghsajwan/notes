### `signed char`
Type for signed character representation.
### `unsigned char`
Type for unsigned character representation.
### `char`
Type for character representation which can be most efficiently processed on the target system.
- `char` type is not same as `signed char` or `unsigned char`.
- It has the same representation as `signed char` or `unsigned char`.
- `sizeof(char)` is always `1`.
- A `char`, a `signed char`, and an `unsigned char` occupy the same amount of storage and have the same alignment requirements.
- The signedness of `char` is implementation defined.
    
    The defaults for ARM and PowerPC are typically unsigned, the defaults for x86 and x64 are typically signed.
    
### `wchar_t`
Type for wide character representation (see [wide strings](https://en.cppreference.com/w/cpp/string/wide)). It has the same size, signedness, and alignment as one of the integer types, but is a distinct type.
- **Size**
    
    `sizeof(wchar_t)` is implementation defined.
    
    It is generally 16 bits in Windows and 32 bits in other platforms.
    
- **Encoding**
    
    Character encoding represented by `whar_t` is also implementation defined.
    
    It generally represents UTF-16 in windows and UTF-32 encoding in other platforms.
    
- **Standard Library Support**
    
    The C++ Standard Library provides various functions and templates for working with wide characters and strings, such as `**std::wcin**`, `**std::wcout**`, and functions like `**std::wstrlen()**` and `**std::wprintf()**`.
    
### `char8_t`
Type for UTF-8 character representation, required to be large enough to represent any UTF-8 code unit (8 bits).
- It has the same size, signedness, and alignment as `unsigned char`, but is a distinct type.
- Since **C++20**.
### `char16_t`
Type for UTF-16 character representation, required to be large enough to represent any UTF-16 code unit (16 bits).
- It has the same size, signedness, and alignment as `[std::uint_least16_t](https://en.cppreference.com/w/cpp/types/integer)`, but is a distinct type.
- Since **C++11**.
### `char32_t`
Type for UTF-32 character representation, required to be large enough to represent any UTF-32 code unit (32 bits).
- It has the same size, signedness, and alignment as `[std::uint_least32_t](https://en.cppreference.com/w/cpp/types/integer)`, but is a distinct type.
- Since in **C++11**.
# See also
Character Literals
# References

> [!info] Why is char distinct from signed char and unsigned char?  
> Consider the following code:  
> [https://stackoverflow.com/questions/16503373/why-is-char-distinct-from-signed-char-and-unsigned-char](https://stackoverflow.com/questions/16503373/why-is-char-distinct-from-signed-char-and-unsigned-char)  

> [!info] char!=(signed char), char!=(unsigned char)  
> The code below compiles, but has different behavior for the char type than for the int types.  
> [https://stackoverflow.com/questions/436513/char-signed-char-char-unsigned-char](https://stackoverflow.com/questions/436513/char-signed-char-char-unsigned-char)  

> [!info] Difference between signed / unsigned char  
> So I know that the difference between a signed int and unsigned int is that a bit is used to signify if the number if positive or negative, but how does this apply to a char?  
> [https://stackoverflow.com/questions/4337217/difference-between-signed-unsigned-char](https://stackoverflow.com/questions/4337217/difference-between-signed-unsigned-char)  

> [!info] conflicts: definition of wchar_t string in C++ standard and Windows implementation?  
> From c++2003 2.  
> [https://stackoverflow.com/questions/4383946/conflicts-definition-of-wchar-t-string-in-c-standard-and-windows-implementati](https://stackoverflow.com/questions/4383946/conflicts-definition-of-wchar-t-string-in-c-standard-and-windows-implementati)  

> [!info] What is the use of wchar_t in general programming?  
> Today I was learning some C++ basics and came to know about wchar_t.  
> [https://stackoverflow.com/questions/13509733/what-is-the-use-of-wchar-t-in-general-programming](https://stackoverflow.com/questions/13509733/what-is-the-use-of-wchar-t-in-general-programming)  

> [!info] Can I turn unsigned char into char and vice versa?  
> I want to use a function that expects data like this:  
> [https://stackoverflow.com/questions/15078638/can-i-turn-unsigned-char-into-char-and-vice-versa](https://stackoverflow.com/questions/15078638/can-i-turn-unsigned-char-into-char-and-vice-versa)  

> [!info] char, wchar_t, char8_t, char16_t, char32_t  
> Learn more about: char, wchar_t, char8_t, char16_t, char32_t  
> [https://learn.microsoft.com/en-us/cpp/cpp/char-wchar-t-char16-t-char32-t?view=msvc-170](https://learn.microsoft.com/en-us/cpp/cpp/char-wchar-t-char16-t-char32-t?view=msvc-170)