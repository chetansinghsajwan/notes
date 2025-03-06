# Object

Git is a content-addressable filesystem. It means that at the core of git is a simple key-value data store.

Git stores all data in form of files, these files are referred as objects. These object are the atoms of a git repo.

They are stored in `.git/objects` directory. This directory is called the [object-store](object-store.md) of git.

There are three types of objects git uses to manage the repo:

1. blob
2. tree
3. commit

To every content git adds a header before creating the object. This header stores the type of the object and the size of the content.

For example,

```
blob 16\u0000
```

Git stores content in a manner similar to a UNIX filesystem, but a bit simplified. All the content is stored as tree and blob objects, with trees corresponding to UNIX directory entries and blobs corresponding more or less to inodes or file contents.

## Blob

A blob object is a simple compressed blob of the content you want to store.

---

**Example**

```shell
$ echo 'test content' | git hash-object -w --stdin
d670460b4b4aece5915caf5c68d12f560a9fe3e4
```

This command returned the hash of the object created in the '.git' directory.

```shell
$ find .git/objects -type f
.git/objects/d6/70460b4b4aece5915caf5c68d12f560a9fe3e4
```

---

**Example**

This will create an object for the content of the file.

**Note:** This will create the object for the contents of the file and not for the file itself. This means that git doesn't store the name of the file, only its content.

```shell
$ echo 'version 1' > test.txt
$ git hash-object -w test.txt
83baae61804e65cc73a7201a7252750c76066a30
```

We can the retrieve the contents of the file.

```shell
$ git cat-file -p 83baae61804e65cc73a7201a7252750c76066a30 > test.txt
$ cat test.txt
version 1
```

---

## Tree

A single tree object contains one or more entries, each of which is the SHA-1 hash of a blob or subtree with its associated mode, type, and filename.

This is where the file name and other properites of the file are stored.

## Commit

A commit object stores:

- hash of root level tree object
- author name
- author email
- author time stamp
- list of hash of parent commits
- commit message
- ...

---

**Example**

```shell
$ git cat-file -p fdf4fc3
tree d8329fc1cc938780ffdd9f94e0d364e0ea74f579
author Scott Chacon <schacon@gmail.com> 1243040974 -0700
committer Scott Chacon <schacon@gmail.com> 1243040974 -0700

First commit
```

---

## References

- https://git-scm.com/book/en/v2/Git-Internals-Git-Objects
