Fundamental Types contains:
- `void` type
- `std::nullptr_t` type
- Arithmetic Types
## `void` type
`void` is used to represent nothing.
- It is an [incomplete type](https://en.cppreference.com/w/cpp/language/incomplete_type) that cannot be completed
- There are no objects of `void`, [arrays](https://en.cppreference.com/w/cpp/language/array) of `void`, nor [references](https://en.cppreference.com/w/cpp/language/reference) to `void`.
- However, [pointers to](https://en.cppreference.com/w/cpp/language/pointer#Pointers_to_void) `[void](https://en.cppreference.com/w/cpp/language/pointer#Pointers_to_void)` and [functions](https://en.cppreference.com/w/cpp/language/function) returning `void` are allowed.
## `std::nullptr_t` type
`std::nullptr_t` is the type of the `nullptr` literal.
- It is a distinct type that is not itself a pointer type or a pointer to member type.
- Its values are _null pointer constant_ (see [NULL](https://en.cppreference.com/w/cpp/types/NULL)).
- Its values can be [implicitly converted](https://en.cppreference.com/w/cpp/language/implicit_cast) to any pointer and pointer to member type.
- `sizeof(std::nullptr_t)` is equal to `sizeof(void*)`.
# Arithmetic Types
Arithmetic Types contains:
- Integer Types
- Floating Point Type
### Integral Types
[[Boolean Type]]
[[Character Types]]
[[Integer Types]]
### Floating Point Types
[[Floating Point Types]]
### Size
Besides the minimal bit counts, the C++ Standard guarantees that
1 == sizeof(char) ≤ sizeof(short) ≤ sizeof(int) ≤ sizeof(long) ≤ sizeof(long long).
Note: this allows the extreme case in which [bytes](https://en.wikipedia.org/wiki/Byte) are sized 64 bits, all types (including char) are 64 bits wide, and [`sizeof`](https://en.cppreference.com/w/cpp/language/sizeof) returns 1 for every type.
# **Data models**
The choices made by each implementation about the sizes of the fundamental types are collectively known as data model.
Four data models found wide acceptance:
- 32 bit systems
    - **LP32** or **2/4/4** (int is 16-bit, long and pointer are 32-bit)
        - Win16 API
    - **ILP32** or **4/4/4** (int, long, and pointer are 32-bit)
        - Win32 API
        - Unix and Unix-like systems (Linux, macOS)
- 64 bit systems
    - **LLP64** or **4/4/8** (int and long are 32-bit, pointer is 64-bit)
        - [Win32 API](https://learn.microsoft.com/en-us/windows/win32/desktop-programming) (also called the Windows API) with compilation target [64-bit ARM](https://en.wikipedia.org/wiki/AArch64) (AArch64) or [x86-64](https://en.wikipedia.org/wiki/x86-64) (a.k.a. x64)
    - **LP64** or **4/8/8** (int is 32-bit, long and pointer are 64-bit)
        - Unix and Unix-like systems (Linux, macOS)
Other models are very rare.
For example,
- **ILP64** (**8/8/8**: int, long, and pointer are 64-bit) only appeared in some early 64-bit Unix systems (e.g. [UNICOS on Cray](https://en.wikipedia.org/wiki/UNICOS)).
  
The following table summarizes all available integer types and their properties in various common data models:
|   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|
|Type specifier|Equivalent type|Width in bits by data model|||||
|||C++ standard|LP32|ILP32|LLP64|LP64|
|signed char|signed char|at least**8**|**8**|**8**|**8**|**8**|
|unsigned char|unsigned char||||||
|short|short int|at least**16**|**16**|**16**|**16**|**16**|
|short int|||||||
|signed short|||||||
|signed short int|||||||
|unsigned short|unsigned short int||||||
|unsigned short int|||||||
|int|int|at least**16**|**16**|**32**|**32**|**32**|
|signed|||||||
|signed int|||||||
|unsigned|unsigned int||||||
|unsigned int|||||||
|long|long int|at least**32**|**32**|**32**|**32**|**64**|
|long int|||||||
|signed long|||||||
|signed long int|||||||
|unsigned long|unsigned long int||||||
|unsigned long int|||||||
|long long|long long int(C++11)|at least**64**|**64**|**64**|**64**|**64**|
|long long int|||||||
|signed long long|||||||
|signed long long int|||||||
|unsigned long long|unsigned long long int(C++11)||||||
|unsigned long long int|||||||
# References

> [!info] Fundamental types - cppreference.com  
> (See also type for type system overview and the list of type-related utilities that are provided by the C++ library)  
> [https://en.cppreference.com/w/cpp/language/types](https://en.cppreference.com/w/cpp/language/types)