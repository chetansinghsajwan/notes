systemd is a system and service manager for Linux operating systems.
- It is the default init system for major linux systems.
- systemd init is the first process started by the linux kernel and is assigned pid 1.
- systemd init runs till system shutdown.
- all other user space processes are started by systemd.
In `systemd`, a unit is a resource that systemd knows how to operate on and manage. This is the primary object that the `systemd` tools use. These resources are defined with configuration files called unit files.
There are 11 types of units:
1. Service units, which start and control daemons and the processes they consist of. For details, see [systemd.service(5)](https://www.freedesktop.org/software/systemd/man/latest/systemd.service.html#).
2. Socket units, which encapsulate local IPC or network sockets in the system, useful for socket-based activation. For details about socket units, see [systemd.socket(5)](https://www.freedesktop.org/software/systemd/man/latest/systemd.socket.html#), for details on socket-based activation and other forms of activation, see [daemon(7)](https://www.freedesktop.org/software/systemd/man/latest/daemon.html#).
3. Target units are useful to group units, or provide well-known synchronization points during boot-up, see [systemd.target(5)](https://www.freedesktop.org/software/systemd/man/latest/systemd.target.html#).
4. Device units expose kernel devices in systemd and may be used to implement device-based activation. For details, see [systemd.device(5)](https://www.freedesktop.org/software/systemd/man/latest/systemd.device.html#).
5. Mount units control mount points in the file system, for details see [systemd.mount(5)](https://www.freedesktop.org/software/systemd/man/latest/systemd.mount.html#).
6. Automount units provide automount capabilities, for on-demand mounting of file systems as well as parallelized boot-up. See [systemd.automount(5)](https://www.freedesktop.org/software/systemd/man/latest/systemd.automount.html#).
7. Timer units are useful for triggering activation of other units based on timers. You may find details in [systemd.timer(5)](https://www.freedesktop.org/software/systemd/man/latest/systemd.timer.html#).
8. Swap units are very similar to mount units and encapsulate memory swap partitions or files of the operating system. They are described in [systemd.swap(5)](https://www.freedesktop.org/software/systemd/man/latest/systemd.swap.html#).
9. Path units may be used to activate other services when file system objects change or are modified. See [systemd.path(5)](https://www.freedesktop.org/software/systemd/man/latest/systemd.path.html#).
10. Slice units may be used to group units which manage system processes (such as service and scope units) in a hierarchical tree for resource management purposes. See [systemd.slice(5)](https://www.freedesktop.org/software/systemd/man/latest/systemd.slice.html#).
11. Scope units are similar to service units, but manage foreign processes instead of starting them as well. See [systemd.scope(5)](https://www.freedesktop.org/software/systemd/man/latest/systemd.scope.html#).
Types of unit states:
|State|Description|
|---|---|
|`active`|Started, bound, plugged in, …, depending on the unit type.|
|`inactive`|Stopped, unbound, unplugged, …, depending on the unit type.|
|`failed`|Similar to `inactive`, but the unit failed in some way (process returned error code on exit, crashed, an operation timed out, or after too many restarts).|
|`activating`|Changing from `inactive` to `active`.|
|`deactivating`|Changing from `active` to `inactive`.|
|`maintenance`|Unit is `inactive` and a maintenance operation is in progress.|
|`reloading`|Unit is `active` and it is reloading its configuration.|
|`refreshing`|Unit is `active` and a new mount is being activated in its namespace.|
# References
- https://www.freedesktop.org/software/systemd/man/latest/systemd.html