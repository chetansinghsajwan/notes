# FHS `/mnt` dir

This directory is provided so that the system administrator may temporarily mount a filesystem as needed. The content of this directory is a local issue and should not affect the manner in which any program is run.

This directory must not be used by installation programs: a suitable temporary directory not in use by the system must be used instead.

## References

- https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html#mntMountPointForATemporarilyMount
