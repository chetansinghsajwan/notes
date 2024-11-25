There are two forms of static initialization:
1. If possible, [constant initialization](https://en.cppreference.com/w/cpp/language/constant_initialization) is applied.
2. Otherwise, non-local static and thread-local variables are [[zero-initialization]].
In practice:x
- Constant initialization is usually applied at compile time.  
    Pre-calculated object representations are stored as part of the program  
    image. If the compiler doesn't do that, it must still guarantee that the initialization happens before any dynamic initialization.
- Variables to be zero-initialized are placed in the `.bss` segment of the program image, which occupies no space on disk and is zeroed out by the OS when loading the program.