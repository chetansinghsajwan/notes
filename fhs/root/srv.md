# FHS `/srv` dir

`/srv` contains site-specific data which is served by this system.

This main purpose of specifying this is so that users may find the location of the data files for a particular service, and so that services which require a single tree for readonly data, writable data and scripts (such as cgi scripts) can be reasonably placed. Data that is only of interest to a specific user should go in that users' home directory. If the directory and file structure of the data is not exposed to consumers, it should go in `/var/lib`.

The methodology used to name subdirectories of `/srv` is unspecified as there is currently no consensus on how this should be done. One method for structuring data under `/srv` is by protocol, eg. `ftp`, `rsync`, `www`, and `cvs`. On large systems it can be useful to structure `/srv` by administrative context, such as `/srv/physics/www`, `/srv/compsci/cvs`, etc. This setup will differ from host to host. Therefore, no program should rely on a specific subdirectory structure of `/srv` existing or data necessarily being stored in `/srv`. However `/srv` should always exist on FHS compliant systems and should be used as the default location for such data.

Distributions must take care not to remove locally placed files in these directories without administrator permission.

## References

- https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html#srvDataForServicesProvidedBySystem
