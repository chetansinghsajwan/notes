# `char` type

###### `signed char`

Type for signed character representation.

###### `unsigned char`

Type for unsigned character representation.

###### `char`

Type for character representation which can be most efficiently processed on the target system.

`char` type is not same as `signed char` or `unsigned char`. But it has the same representation as `signed char` or `unsigned char`.

Size of `char` is always `1`.

The signedness of `char` is implementation defined. The defaults for ARM and PowerPC are typically unsigned, the defaults for x86 and x64 are typically signed.

A `char`, a `signed char`, and an `unsigned char` occupy the same amount of storage and have the same alignment requirements.

###### `wchar_t`

Type for wide character representation (see [wide strings](https://en.cppreference.com/w/cpp/string/wide)). It has the same size, signedness, and alignment as one of the integer types, but is a distinct type.

**Size:**

`sizeof(wchar_t)` is implementation defined. It is generally 16 bits in Windows and 32 bits in other platforms.

**Encoding:**

Character encoding represented by `whar_t` is also implementation defined. It generally represents UTF-16 in windows and UTF-32 encoding in other platforms.

**Standard Library Support:**

The C++ Standard Library provides various functions and templates for working with wide characters and strings, such as `**std::wcin**`, `**std::wcout**`, and functions like `**std::wstrlen()**` and `**std::wprintf()**`.

###### `char8_t`

**since:** cpp20

Type for UTF-8 character representation, required to be large enough to represent any UTF-8 code unit (8 bits).

It has the same size, signedness, and alignment as `unsigned char`, but is a distinct type.

###### `char16_t`

**since**: cpp11

Type for utf16 character representation, required to be large enough to represent any UTF-16 code unit (16 bits).

It has the same size, signedness, and alignment as `[std::uint_least16_t](https://en.cppreference.com/w/cpp/types/integer)`, but is a distinct type.

###### `char32_t`

**since**: cpp11

Type for utf32 character representation, required to be large enough to represent any UTF-32 code unit (32 bits).

It has the same size, signedness, and alignment as `[std::uint_least32_t](https://en.cppreference.com/w/cpp/types/integer)`, but is a distinct type.

## References

- https://twiserandom.com/cpp/cpp-character-types-char-wchar_t-char8_t-char16_t-and-char32_t-a-tutorial/index.html

- https://stackoverflow.com/questions/16503373/why-is-char-distinct-from-signed-char-and-unsigned-char

- https://stackoverflow.com/questions/436513/char-signed-char-char-unsigned-char

- https://stackoverflow.com/questions/4337217/difference-between-signed-unsigned-char

- https://stackoverflow.com/questions/4383946/conflicts-definition-of-wchar-t-string-in-c-standard-and-windows-implementati

- https://stackoverflow.com/questions/13509733/what-is-the-use-of-wchar-t-in-general-programming

- https://stackoverflow.com/questions/15078638/can-i-turn-unsigned-char-into-char-and-vice-versa

- https://learn.microsoft.com/en-us/cpp/cpp/char-wchar-t-char16-t-char32-t?view=msvc-170