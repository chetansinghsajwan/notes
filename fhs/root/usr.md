# FHS `/usr` heirarchy

The following directories, or symbolic links to directories, are required in `/usr`.

- `bin`: Most user commands
- `lib`: Libraries
- `local`: Local hierarchy (empty after main installation)
- `sbin`: Non-vital system binaries
- `share`: Architecture-independent data

The following directories, or symbolic links to directories, must be in `/usr`, if the corresponding subsystem is installed:

- `games`: Games and educational binaries (optional)
- `include`: Header files included by C programs
- `libexec`: Binaries run by other programs (optional)
- `lib<qual>`: Alternate Format Libraries (optional)
- `src`: Source code (optional)

## References

- https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch04.html
