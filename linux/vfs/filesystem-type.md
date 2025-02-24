# VFS Filesystem Type

In order to use a filesystem, the kernel must know the filesystem type, defined in [include/linux/fs.h](https://elixir.bootlin.com/linux/v5.7-rc4/source/include/linux/fs.h#L2234) (and shorted here for brevity):

```cpp
struct file_system_type {
  const char* name; // friendly name of the filesystem -- like 'ext2'
  int fs_flags; // mostly obscure options
  int(*init_fs_context)(struct fs_context* );
  const struct fs_parameter_spec* parameters;
  struct dentry* (*mount)(struct file_system_type* , int, const char*, void*);
  void (*kill_sb)(struct super_block*);
  struct module* owner;
  struct file_system_type* next;
  struct hlist_head fs_supers;
};
```

Note: All data structures mentioned here are shown as they were in Linux [v5.18.6](https://elixir.bootlin.com/linux/v5.18.6/source).

`struct file_system_type` maintains data related to a filesystem but is not associated with any particular 'instance of a filesystem', a phrase which here means a mounted filesystem. Examples of filesystems include:

- `ext2/3/4`
- `fat16/32`
- `ntfs`
- `btrfs`
- `isofs`
- `reiserfs`
- `squashfs`
- ... and the list goes on.

The VFS maintains a linked-list of known filesystem types, which can be viewed in userspace by executing `cat /proc/filesystems`.

The member `struct hlist_head fs_supers`  is a hash table of all [superblocks](/linux/vfs/superblock) of the same filesystem type. This means that `fs_supers` provides a list of all mounts of this `file_system_type`.

Filesystem types are registered and unregistered in the kernel via [these functions in include/linux/fs.h](https://elixir.bootlin.com/linux/v5.7-rc4/source/include/linux/fs.h#L2325):

```cpp
int register_filesystem(struct file_system_type*);
int unregister_filesystem(struct file_system_type*);
```

Once a filesystem type is known to the kernel (i.e. registered), a filesystem of that type may be mounted to a location in the directory tree.

## References

- https://www.starlab.io/blog/introduction-to-the-linux-virtual-filesystem-vfs-part-i-a-high-level-tour
