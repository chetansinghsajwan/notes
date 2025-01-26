---
status: completed
---

# Linux VFS

VFS stands for Virtual File System.

It is not an actual file system, instead an abstraction layer that hides the underlying file system.

This allows the user to open a file by calling `open` function without worrying about the file system that file is being managed by.

This layer is used by the kernel itself to intereact with different filesystems. The kernel then exposes system calls like `open` function which allows programs in user space to interact with files.

## References

- https://docs.kernel.org/filesystems/vfs.html
- https://www.starlab.io/blog/introduction-to-the-linux-virtual-filesystem-vfs-part-i-a-high-level-tour

## Read Later

- https://dl.acm.org/doi/fullHtml/10.5555/328437.328458
