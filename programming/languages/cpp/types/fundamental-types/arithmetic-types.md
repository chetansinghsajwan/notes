# Arithmetic Types

Arithmetic Types contains:

- Integer Types
- Floating Point Type

## Size

Besides the minimal bit counts, the C++ Standard guarantees that
1 == sizeof(char) ≤ sizeof(short) ≤ sizeof(int) ≤ sizeof(long) ≤ sizeof(long long).

Note: this allows the extreme case in which [bytes](https://en.wikipedia.org/wiki/Byte) are sized 64 bits, all types (including char) are 64 bits wide, and [`sizeof`](https://en.cppreference.com/w/cpp/language/sizeof) returns 1 for every type.

## **Data models**

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

**Example:**

**ILP64** (**8/8/8**: int, long, and pointer are 64-bit) only appeared in some early 64-bit Unix systems (e.g. [UNICOS on Cray](https://en.wikipedia.org/wiki/UNICOS)).

## References

- https://en.cppreference.com/w/cpp/language/types