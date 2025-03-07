# Git Index

Also known as **Staging Area**.

The index is a binary file (`.git/index`) that stores a sorted list of file names, along with file metadata and pointers to the object database—the files that contain the contents of these files at the time they were added to the index.

You can refer to [Git - index-format Documentation](https://git-scm.com/docs/index-format) to learn about the index file format.

This is used to track Working Directory changes, that have been promoted to be stored in the next commit.

This allows to commit only some of the changes you made.

## References

- https://git-scm.com/about/staging-area
- https://graphite.dev/guides/git-index
- https://git-scm.com/docs/index-format
