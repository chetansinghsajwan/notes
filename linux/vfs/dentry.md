# VFS Dentry

Dentry stands directory entry.

A dentry is the glue that holds inodes and files together by relating inode numbers to file names. The struct inode itself does not contain a name or any friendly identifier, this is where dentry comes in. It assigns inode objects a friendly name in a tree like structure.

The dentry structure is defined in [include/linux/dcache.h](https://elixir.bootlin.com/linux/v5.7-rc4/source/include/linux/dcache.h#L89) (and shortened here for brevity):

```cpp
struct dentry {
  struct hlist_bl_node d_hash; // lookup hash list
  struct dentry * d_parent; // parent directory
  struct qstr d_name;
  struct inode * d_inode; // w here the name belongs to
  unsigned char d_iname[DNAME_INLINE_LEN]; // small names
  const struct dentry_operations * d_op;
  void * d_fsdata; // fs-specific data
  struct list_head d_child; // child of parent list, i.e., our siblings
  struct list_head d_subdirs; // our children
};
```

`d_name` contains the name of the file and `d_inode` points to its associated inode.

## Refefences

- https://www.starlab.io/blog/introduction-to-the-linux-virtual-filesystem-vfs-part-i-a-high-level-tour
