# Filesystem Journaling

All the above file systems with the exception of exFAT, ext2, FAT16/32, Reiser4 (optional), Bcachefs, Btrfs, and ZFS, use [journaling](https://en.wikipedia.org/wiki/Journaling_file_system "wikipedia:Journaling file system"). Journaling provides fault-resilience by logging changes before they are committed to the file system. In the event of a system crash or power failure, such file systems are faster to bring back online and less likely to become corrupted. The logging takes place in a dedicated area of the file system.

Not all journaling techniques are the same. Ext3 and ext4 offer data-mode journaling, which logs both data and meta-data, as well as possibility to journal only meta-data changes. Data-mode journaling comes with a speed penalty and is not enabled by default.

The other file systems provide ordered-mode journaling, which only logs meta-data. While all journaling will return a file system to a valid state after a crash, data-mode journaling offers the greatest protection against corruption and data loss

## References

- https://wiki.archlinux.org/title/File_systems
