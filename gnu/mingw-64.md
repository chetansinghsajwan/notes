# Mingw-w64

Mingw-w64 is an advancement of the original mingw.org project.

Mingw-w64 can generate 32 bit and 64-bit executables.

MinGW-w64 provides

- A more complete Win32 implementation
- [Win64](https://en.wikipedia.org/wiki/Win64) support
- Better [C99](https://en.wikipedia.org/wiki/C99) support
- [POSIX Threads](https://en.wikipedia.org/wiki/POSIX_Threads) (pthreads) support (including the possibility to enable [C++11](https://en.wikipedia.org/wiki/C%2B%2B11) thread-related functionality in GCC's [libstdc++](https://en.wikipedia.org/wiki/Libstdc%2B%2B))
- GCC multilib, which allows users to install 32-bit and 64-bit libraries in parallel
- [Unicode](https://en.wikipedia.org/wiki/Unicode_in_Microsoft_Windows) entry point (wmain/wWinMain)
- [DDK](https://en.wikipedia.org/wiki/Windows_Driver_Kit) (from [ReactOS](https://en.wikipedia.org/wiki/ReactOS))
- [DirectX](https://en.wikipedia.org/wiki/DirectX) (from [Wine](https://en.wikipedia.org/wiki/Wine_(software)))
- [Large file support](https://en.wikipedia.org/wiki/Large_file_support)
- [Structured Exception Handling](https://en.wikipedia.org/wiki/Structured_Exception_Handling) (SEH) instead of [DWARF](https://en.wikipedia.org/wiki/DWARF) or [sjlj](https://en.wikipedia.org/wiki/Setjmp) on x86-64 (from gcc 4.8+)
- Some useful tools such as `gendef` (an improved version of MinGW's `pexports` utility), and `widl` (an [MIDL](https://en.wikipedia.org/wiki/MIDL) compiler from Wine).

Additionally, the Mingw-w64 project maintains winpthreads, a [wrapper library](https://en.wikipedia.org/wiki/Wrapper_library) similar to pthreads-win32, with the main difference that it allows GCC to use it as a threads library resulting in functional C++11 thread libraries `<thread>`, `<future>`, and `<mutex>`.

## History

- In 2005, Mingw-w64 was created by OneVision Software under [cleanroom software engineering](https://en.wikipedia.org/wiki/Cleanroom_software_engineering) principles, since the original MinGW project was not prompt on updating its code base, including the inclusion of several key new APIs and also much needed 64-bit support.
- In 2008, OneVision then donated the code to Kai Tietz, one of its lead developers, under the condition that it remains open source.[[1]](https://en.wikipedia.org/wiki/Mingw-w64\#cite_note-MinGW-w64_History-1)
- It was first submitted to the original MinGW project, but refused under suspicion of using non-public or proprietary information.[[2]](https://en.wikipedia.org/wiki/Mingw-w64\#cite_note-2)[[1]](https://en.wikipedia.org/wiki/Mingw-w64#cite_note-MinGW-w64_History-1)[[3]](https://en.wikipedia.org/wiki/Mingw-w64#cite_note-3) For many reasons, the lead developer and co-founder of the MinGW-w64 project, Kai Tietz, decided not to attempt further cooperation with MinGW.[[4]](https://en.wikipedia.org/wiki/Mingw-w64\#cite_note-4)

## Distribution

The MinGW-w64 project does not provide official binary builds: These can be grabbed either from the personal build directories of the developers (the most popular being rubenvb), or from associated but independent projects like tdm-gcc or mingw-builds or msys2.

## References

- https://sourceforge.net/p/mingw-w64/wiki2/Home/

- https://en.wikipedia.org/wiki/Mingw-w64?wprov=sfla1

- https://www.mingw-w64.org

- https://mingwpy.github.io/background-mingw.html

- https://wiki.qt.io/Mingw-64-bit