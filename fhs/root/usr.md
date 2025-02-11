---
status: completed
---

# FHS `/usr` heirarchy

It contains read-only data that is shared among all the users. It is the second major section of the filesystem.

Really good explanation about why `/usr` dir exists: https://askubuntu.com/questions/130186/what-is-the-rationale-for-the-usr-directory.

Checkout the case for `/usr` merge: https://systemd.io/THE_CASE_FOR_THE_USR_MERGE.

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

The following symbolic links to directories may be present. This possibility is based on the need to preserve compatibility with older systems until all distribution can be assumed to use the `/var` hierarchy.

- `usr/spool` -> `/var/spool`
- `/usr/tmp` -> `/var/tmp`
- `/usr/spool/locks` -> `/var/lock`

## References

- https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch04.html
- https://askubuntu.com/questions/130186/what-is-the-rationale-for-the-usr-directory
