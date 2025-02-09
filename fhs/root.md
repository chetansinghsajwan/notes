---
status: completed
---

# FHS `/`

The contents of the root filesystem must be adequate to boot, restore, recover, and/or repair the system.

- To boot a system, enough software and data must be present on the root partition to mount other filesystems. This includes utilities, configuration, boot loader information, and other essential start-up data. `/usr`, `/opt`, and `/var` are designed such that they may be located on other partitions or filesystems.

- To enable recovery and/or repair of a system, those utilities needed by an experienced maintainer to diagnose and reconstruct a damaged system must be present on the root filesystem.

- To restore a system, those utilities needed to restore from system backups (on floppy, tape, etc.) must be present on the root filesystem.

The following directories, or symbolic links to directories, are required in `/`.

- `bin`: Essential command binaries
- `boot`: Static files of the boot loader
- `dev`: Device files
- `etc`: Host-specific system configuration
- `lib`: Essential shared libraries and kernel modules
- `media`: Mount point for removable media
- `mnt`: Mount point for mounting a filesystem temporarily
- `opt`: Add-on application software packages
- `run`: Data relevant to running processes
- `sbin`: Essential system binaries
- `srv`: Data for services provided by this system
- `tmp`: Temporary files
- `usr`: Secondary hierarchy
- `var`: Variable data
## References

- https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch03.html
