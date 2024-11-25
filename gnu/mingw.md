# Mingw

MinGW stands for **Minimalist GNU for Windows**.
It is a port of GNU environment for development on Windows.

## Components

The MinGW project maintains and distributes a number of different core components and supplementary packages. These includes:
- **GNU Compiler Collection**
    
    Contains compilers for various programming languages like [C](https://en.wikipedia.org/wiki/C_(programming_language)), [C++](https://en.wikipedia.org/wiki/C%2B%2B), [Objective-C](https://en.wikipedia.org/wiki/Objective-C), [Objective-C++](https://en.wikipedia.org/wiki/Objective-C%2B%2B), [Fortran](https://en.wikipedia.org/wiki/Fortran), and [Ada](https://en.wikipedia.org/wiki/Ada_(programming_language)).
    
- **GNU Debugger**
    
    Debugger `gdb` for debugging C and C++ programs.
    
- **GNU Binutils**
    
    Collection of binary utilities that includes tools for working with object files, executable files, and libraries. It includes utilities like `as` (assembler), `ld` (linker), and `objcopy` (object file copy).
    
- **MSYS**
    
    Provides a minimal Unix-like shell environment on Windows. It allows you to run Unix-like commands and scripts, which can be helpful for building and configuring software.
    
- **Headers and Libraries**
    
    Provides header files and import libraries for the Windows API, allowing you to develop Windows applications. It also includes standard C and C++ libraries.
    
    Mingw uses the Microsoft runtime libraries, distributed with the Windows operating system. Unlike other ports of gcc to Windows, the runtime libraries are not distributed using Gnu's General Public License. You, therefore, do not have to distribute your source code with your programs.
    
- **Development Tools**
    
    Tools and utilities for managing and building software, such as `make`, `autoconf`, and `automake`.
    
## Programming language support
Most languages supported by GCC are supported on the MinGW port as well. These include [C](https://en.wikipedia.org/wiki/C_(programming_language)), [C++](https://en.wikipedia.org/wiki/C%2B%2B), [Objective-C](https://en.wikipedia.org/wiki/Objective-C), [Objective-C++](https://en.wikipedia.org/wiki/Objective-C%2B%2B), [Fortran](https://en.wikipedia.org/wiki/Fortran), and [Ada](https://en.wikipedia.org/wiki/Ada_(programming_language)).
- The GCC runtime libraries are used (libstdc++ for C++, libgfortran for Fortran, etc.).

## Platform Availability

MinGW can be run either on Windows platform, cross-hosted on Linux (or other Unix), or "cross-native" on Cygwin. Although programs produced under MinGW are 32-bit executables, they can be used both in 32 and 64-bit versions of Windows.

## Link compatibility

Binaries (executables or DLLs) generated with different C++ compilers (like MinGW and Visual Studio) are in general not link compatible due to different ABI. However, compiled C code is link compatible.
See: [https://stackoverflow.com/questions/7119588/ms-vs-non-ms-c-compiler-compatibility](https://stackoverflow.com/questions/7119588/ms-vs-non-ms-c-compiler-compatibility)

## History

- MinGW was originally called mingw32 ("Minimalist GNU for Win32"). The numbers were dropped in order to avoid the implication that it would be limited to producing [32-bit binaries](https://en.wikipedia.org/wiki/32-bit_application).
- **Colin Peters** authored the initial release in 1998, consisting only of a Cygwin port of GCC.
- **Jan-Jaap van der Heijden** created a Windows-native port of GCC and added [binutils](https://en.wikipedia.org/wiki/Binutils) and [make](https://en.wikipedia.org/wiki/Make_(software)).
- **Mumit Khan** later took over development, adding more Windows-specific features to the package, including the Windows system headers by Anders Norlander.
- In 2000, the project was moved to [SourceForge](https://en.wikipedia.org/wiki/SourceForge) in order to get assistance from the community and centralize its development.
- MinGW was selected as Project of the Month at SourceForge for September 2005.
- MSYS (a contraction of "Minimal System") was introduced as a [Bourne shell](https://en.wikipedia.org/wiki/Bourne_shell) command line interpreter system with the aim of better interoperability with native Windows software.
- In 2018, following a disagreement with SourceForge about the administration of its mailing lists, MinGW migrated to [OSDN](https://en.wikipedia.org/wiki/OSDN).
- In 2007, a fork of the original MinGW called [Mingw-w64](https://en.wikipedia.org/wiki/Mingw-w64) appeared in order to provide support for 64 bits and new APIs.

## References

- https://en.wikipedia.org/wiki/MinGW

> > [!todo]
> > 
> > Fix this broken link.
>  
> http://mingw.osdn.io/index.html

- https://home.cs.colorado.edu/~main/cs1300/doc/mingwfaq.html

- https://www.sobyte.net/post/2021-11/cygwin-mingw-msys