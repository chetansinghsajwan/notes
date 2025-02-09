# FHS `/etc` dir

The `/etc` hierarchy contains configuration files. A "configuration file" is a local file used to control the operation of a program; it must be static and cannot be an executable binary. [[2]](https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html#ftn.idm236088529392)

It is recommended that files be stored in subdirectories of `/etc` rather than directly in `/etc`.

No binaries may be located under `/etc`.

The following directories, or symbolic links to directories are required in `/etc`:

| Directory | Description            |
| :-------- | :--------------------- |
| opt       | Configuration for /opt |
## References

- https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html#etcHostspecificSystemConfiguration
