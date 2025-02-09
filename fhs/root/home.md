# FHS `/home` dir

User specific configuration files for applications are stored in the user's home directory in a file that starts with the '.' character (a "dot file"). If an application needs to create more than one dot file then they should be placed in a subdirectory with a name starting with a '.' character, (a "dot directory"). In this case the configuration files should not start with the '.' character.

## References

- https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html#homeUserHomeDirectories
