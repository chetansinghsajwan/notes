# VFS Inode

Inode stands for Index node.

It represents a file system object, which can be any of the following:
- socket
- symbolic link
- regular file
- block device
- directory
- character device
- FIFO

It is defined in [include/linux/fs.h](https://elixir.bootlin.com/linux/v5.7-rc4/source/include/linux/fs.h#L633) (and shortened here for brevity):

```cpp
struct inode {
  umode_t i_mode; // access permissions, i.e., readable or writeable     
  kuid_t i_uid; // user id of owner     
  kgid_t i_gid; // group id of owner     
  unsigned int i_flags;
  const struct inode_operations * i_op;
  struct super_block * i_sb;
  struct address_space * i_mapping;
  unsigned long i_ino; // unique number identifying this inode     
  const unsigned int i_nlink; // number of hard links     
  dev_t i_rdev;
  loff_t i_size; // size of inode contents in bytes     struct
  timespec64 i_atime; // access time     
  struct timespec64 i_mtime; // modify time     
  struct timespec64 i_ctime; // creation time
  unsigned short i_bytes; // bytes consumed
  const struct file_operations * i_fop;
  struct address_space i_data;
};
```

Note the fields [i_op](https://elixir.bootlin.com/linux/v5.7-rc4/source/include/linux/fs.h#L1868) and [i_fop](https://elixir.bootlin.com/linux/v5.7-rc4/source/include/linux/fs.h#L1826). These are function pointers that describe the filesystem’s implementation. `struct inode_operations` defines the set of callback functions that operate on the inode. These functions do things like:

- change permissions
- create files
- make symlinks
- make directories    
- rename files

Similarly, `struct file_operations` defines the set of callback operations that can be called on a `struct file` object.

## References

- https://www.starlab.io/blog/introduction-to-the-linux-virtual-filesystem-vfs-part-i-a-high-level-tour
