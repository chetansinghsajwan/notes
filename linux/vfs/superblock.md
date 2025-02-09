# VFS Superblock

The superblock is a structure that represents an instance of a filesystem, i.e., a mounted filesystem. The superblock is defined in [include/linux/fs.hs](https://elixir.bootlin.com/linux/v5.7-rc4/source/include/linux/fs.h#L1430) (and greatly shortened here for brevity):

```cpp
struct super_block {
  struct list_head s_list; // list of all other superblocks of this filesystem type
  dev_t s_dev; // device associated with this mount
  unsigned long s_blocksize;
  loff_t s_maxbytes;
  struct file_system_type * s_type; // struct describing the type of filesystem this mount represents
  const struct super_operations * s_op;
  uuid_t s_uuid; // unique ID of this mount
  struct list_head s_inodes;
  unsigned long s_magic; // magic number of this filesystem
  struct dentry * s_root;
  int s_count;
  void * s_fs_info;
  const struct dentry_operations * s_d_op;
};
```

Learn about [magic numbers here](https://superuser.com/questions/239088/whats-a-file-systems-magic-number-in-a-super-block).

The superblock is typically stored on the storage device itself and loaded into memory when mounted.

There are a few fields we want to pay special attention to.

- `s_list` is a linked list to the other superblocks of the same filesystem type.
- `s_inodes` is the list of [inodes](/linux/vfs/inode) within this filesystem mount.
- [s_op](https://elixir.bootlin.com/linux/v5.7-rc4/source/include/linux/fs.h#L1947), which points to a struct that defines a set of functions that provide data about the superblock. `struct super_operations` is a prime example of the VFS’s abstraction that we will see again. It contains a group of function pointers provided per filesystem type that describe the filesystem’s implementation. This keeps the VFS agnostic to the details of a particular filesystem’s inner workings.

## References

- https://www.starlab.io/blog/introduction-to-the-linux-virtual-filesystem-vfs-part-i-a-high-level-tour
