# FHS `/lib<qual>` dir

There may be one or more variants of the `/lib` directory on systems which support more than one binary format requiring separate libraries.

If one or more of these directories exist, the requirements for their contents are the same as the normal `/lib` directory, except that ``/lib_`<qual>`/cpp`` is not required.

## References

- https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html#libltqualgtAlternateFormatEssential
