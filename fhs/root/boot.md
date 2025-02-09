---
status: completed
---

# FHS `/boot` dir

This directory contains everything required for the boot process, except configuration files not needed at boot time and the map installer. Thus `/boot` stores data that is used before the kernel begins executing user-mode programs.

Programs necessary to arrange for the boot loader to be able to boot a file must be placed in `/sbin`. Configuration files for boot loaders that are not required at boot time must be placed in `/etc`.

The operating system kernel must be located in either `/` or `/boot`.

## References

- https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html#bootStaticFilesOfTheBootLoader
