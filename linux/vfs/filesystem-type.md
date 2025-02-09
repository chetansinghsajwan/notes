# VFS Filesystem Type

In order to use a filesystem, the kernel must know the filesystem type, defined in [include/linux/fs.h](https://elixir.bootlin.com/linux/v5.7-rc4/source/include/linux/fs.h#L2234) (and shorted here for brevity):

```
struct file_system_type {
  const char* name;      // friendly name of the filesystem -- like 'ext2'
  int fs_flags;   // mostly obscure options
  int (*init_fs_context)(struct fs_context*);
  const struct fs_parameter_spec  *parameters;
  struct dentry* (*mount) (struct file_system_type*, int, const char *, void *);     void                            (*kill_sb) (struct super_block *);     struct module                   *owner;     struct file_system_type         *next;     struct hlist_head               fs_supers; };
```