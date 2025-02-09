---
status: completed
---

# FHS `/dev` dir

The `/dev` directory is the location of special or device files.

If it is possible that devices in `/dev` will need to be manually created, `/dev` must contain a command named `MAKEDEV`, which can create devices as needed. It may also contain a `MAKEDEV.local` for any local devices.

If required, `MAKEDEV` must have provisions for creating any device that may be found on the system, not just those that a particular distribution installs.

Note, 

## References

- https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html#devDeviceFiles
