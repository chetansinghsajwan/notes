# VFS File

A file object represents an open file and is defined in [include/linux/fs.h](https://elixir.bootlin.com/linux/v5.7-rc4/source/include/linux/fs.h#L941) (and shortened here for brevity):

```cpp
struct file {
  struct path f_path; // a dentry and a mount point which locate this file     
  struct inode * f_inode; // the inode underlying this file     
  const struct file_operations * f_op; // callbacks to function which can operate on this file 
  spinlock_t f_lock;
  atomic_long_t f_count;
  unsigned int f_flags;
  fmode_t f_mode;
  struct mutex f_pos_lock;
  loff_t f_pos // offset in the file from which the next read or write shall commence     
  struct fown_struct f_owner;
  void * private_data struct address_space * f_mapping; // callbacks for memory mapping operations 
};
```

## References

- https://www.starlab.io/blog/introduction-to-the-linux-virtual-filesystem-vfs-part-i-a-high-level-tour
