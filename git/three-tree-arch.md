# The Three Tree Architecture

By “tree” here, we really mean “collection of files”, not specifically the data structure. There are a few cases where the index doesn’t exactly act like a tree, but for our purposes it is easier to think about it this way for now.

Git as a system manages and manipulates three trees in its normal operation:

## Head

HEAD is the pointer to the current branch reference, which is in turn a pointer to the last commit made on that branch.

## Index

Also known as **Staging Area**.
This tree is tracking Working Directory changes, that have been promoted with `git add`, to be stored in the next commit. This tree is a complex internal caching mechanism.

## Working Tree

Working Tree is the root folder, where you work. This is where all your current changes are.

## Why git follows three tree Architecture?

## References

- https://git-scm.com/book/en/v2/Git-Tools-Reset-Demystified
