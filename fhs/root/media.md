# FHS `/media` dir

This directory contains subdirectories which are used as mount points for removable media such as floppy disks, cdroms and zip disks.

Historically there have been a number of other different places used to mount removable media such as `/cdrom`, `/mnt` or `/mnt/cdrom`. Placing the mount points for all removable media directly in the root directory would potentially result in a large number of extra directories in `/`. Although the use of subdirectories in `/mnt` as a mount point has recently been common, it conflicts with a much older tradition of using `/mnt` directly as a temporary mount point.

The following directories, or symbolic links to directories, must be in `/media`, if the corresponding subsystem is installed:

- `floppy`: Floppy drive (optional)
- `cdrom`: CD-ROM drive (optional)
- `cdrecorder`: CD writer (optional)
- `zip`: Zip drive (optional)

On systems where more than one device exists for mounting a certain type of media, mount directories can be created by appending a digit to the name of those available above starting with '0', but the unqualified name must also exist.

For example, a compliant distribution with two CDROM drives might have `/media/cdrom0` and `/media/cdrom1` with `/media/cdrom` a symlink to either of these.

## References

- https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html#mediaMountPoint
