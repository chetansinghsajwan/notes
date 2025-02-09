---
status: completed
---

# FHS `/bin` dir

`/bin` contains commands that may be used by both the system administrator and by users, but which are required when no other filesystems are mounted (e.g. in single user mode). It may also contain commands which are used indirectly by scripts.

There must be no subdirectories in `/bin`.

The following commands, or symbolic links to commands, are required in `/bin`:

- `cat`: Utility to concatenate files to standard output
- `chgrp`: Utility to change file group ownership
- `chmod`: Utility to change file access permissions
- `chown`: Utility to change file owner and group
- `cp`: Utility to copy files and directories
- `date`: Utility to print or set the system data and time
- `dd`: Utility to convert and copy a file
- `df`: Utility to report filesystem disk space usage
- `dmesg`: Utility to print or control the kernel message buffer
- `echo`: Utility to display a line of text
- `false`: Utility to do nothing, unsuccessfully
- `hostname`: Utility to show or set the system's host name
- `kill`: Utility to send signals to processes
- `ln`: Utility to make links between files
- `login`: Utility to begin a session on the system
- `ls`: Utility to list directory contents
- `mkdir`: Utility to make directories
- `mknod`: Utility to make block or character special files
- `more`: Utility to page through text
- `mount`: Utility to mount a filesystem
- `mv`: Utility to move/rename files
- `ps`: Utility to report process status
- `pwd`: Utility to print name of current working directory
- `rm`: Utility to remove files or directories
- `rmdir`: Utility to remove empty directories
- `sed`: The `sed' stream editor
- `sh`: POSIX compatible command shell
- `stty`: Utility to change and print terminal line settings
- `su`: Utility to change user ID
- `sync`: Utility to flush filesystem buffers
- `true`: Utility to do nothing, successfully
- `umount`: Utility to unmount file systems
- `uname`: Utility to print system information

If `/bin/sh` is not the POSIX compatible shell command itself, it must be a hard or symbolic link to the real shell command.

The following programs, or symbolic links to programs, must be in `/bin` if the corresponding subsystem is installed:

- `csh`: The C shell (optional)
- `ed`: The `ed' editor (optional)
- `tar`: The tar archiving utility (optional)
- `cpio`: The cpio archiving utility (optional)
- `gzip`: The GNU compression utility (optional)
- `gunzip`: The GNU uncompression utility (optional)
- `zcat`: The GNU uncompression utility (optional)
- `netstat`: The network statistics utility (optional)
- `ping`: The ICMP network test utility (optional)

`/bin/csh` may be a symbolic link to `/bin/tcsh` or `/usr/bin/tcsh`.

## References

- https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html#binEssentialUserCommandBinaries
