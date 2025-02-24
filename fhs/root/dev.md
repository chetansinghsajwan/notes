# FHS `/dev` dir

The `/dev` directory is the location of special or device files.

If it is possible that devices in `/dev` will need to be manually created, `/dev` must contain a command named `MAKEDEV`, which can create devices as needed. It may also contain a `MAKEDEV.local` for any local devices.

If required, `MAKEDEV` must have provisions for creating any device that may be found on the system, not just those that a particular distribution installs.

Note, `MAKEDEV` script does not exist in most distributions anymore. This is described in this answer at stack overflow: https://unix.stackexchange.com/questions/396038/why-cant-i-find-makedev-in-the-dev-folder.

## References

- https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html#devDeviceFiles
- https://unix.stackexchange.com/questions/396038/why-cant-i-find-makedev-in-the-dev-folder
