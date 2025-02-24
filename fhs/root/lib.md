# FHS `/lib` dir

The `/lib` directory contains those shared library images needed to boot the system and run the commands in the root filesystem, ie. by binaries in `/bin` and `/sbin`.

At least one of each of the following filename patterns are required (they may be files, or symbolic links):

- `libc.so.*`: The dynamically-linked C library (optional)
- `ld*`: The execution time linker/loader (optional)

If a C preprocessor is installed, `/lib/cpp` must be a reference to it, for historical reasons.

The following directories, or symbolic links to directories, must be in `/lib`, if the corresponding subsystem is installed:

- `modules`: Loadable kernel modules (optional)

## References

- https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html#libEssentialSharedLibrariesAndKern
