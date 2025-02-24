# FHS `sbin` dir

`/sbin` contains binaries essential for booting, restoring, recovering, and/or repairing the system in addition to the binaries in `/bin`.

Utilities used for system administration (and other root-only commands) are stored in `/sbin`, `/usr/sbin`, and `/usr/local/sbin`.

Programs executed after `/usr` is known to be mounted (when there are no problems) are generally placed into `/usr/sbin`. Locally-installed system administration programs should be placed into `/usr/local/sbin`.

There must be no subdirectories in `/sbin`.

The following commands, or symbolic links to commands, are required in `/sbin`:

- `shutdown`: Command to bring the system down.

The following files, or symbolic links to files, must be in `/sbin` if the corresponding subsystem is installed:

- `fastboot`: Reboot the system without checking the disks (optional).
- `fasthalt`: Stop the system without checking the disks (optional).
- `fdisk`: Partition table manipulator (optional).
- `fsck`: File system check and repair utility (optional).
- `fsck.*`: File system check and repair utility for a specific filesystem (optional).
- `getty`: The getty program (optional).
- `halt`: Command to stop the system (optional).
- `ifconfig`: Configure a network interface (optional).
- `init`: Initial process (optional).
- `mkfs`: Command to build a filesystem (optional).
- `mkfs.*`: Command to build a specific filesystem (optional).
- `mkswap`: Command to set up a swap area (optional).
- `reboot`: Command to reboot the system (optional).
- `route`: IP routing table utility (optional).
- `swapon`: Enable paging and swapping (optional).
- `swapoff`: Disable paging and swapping (optional).
- `update`: Daemon to periodically flush filesystem buffers (optional).

## References

- https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html#sbinSystemBinaries
