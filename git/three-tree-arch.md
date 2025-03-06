# The Three Tree Architecture

By “tree” here, we really mean “collection of files”, not specifically the data structure. There are a few cases where the index doesn’t exactly act like a tree, but for our purposes it is easier to think about it this way for now.

Git as a system manages and manipulates three trees in its normal operation:

## Head

Head is just a reference to a commit. Git uses this reference to determine the current state of the repository.

For example, Git uses head to compare Head Tree with the working tree to determine what are the changes made in the working tree, that is, how is working tree different from the commit head points to.

## Index

Also known as **Staging Area**.

This tree is used to track Working Directory changes, that have been promoted to be stored in the next commit.

## Working Tree

Working Tree is the root folder, where you work. This is where all your current changes are.

## Why git follows three tree Architecture?

## References

- https://git-scm.com/book/en/v2/Git-Tools-Reset-Demystified
