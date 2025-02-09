---
status: completed
---

# FHS `/tmp` dir

The `/tmp` directory must be made available for programs that require temporary files.

Programs must not assume that any files or directories in `/tmp` are preserved between invocations of the program.

## References

- https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html#tmpTemporaryFiles
