# Tag

Tags are just [references](ref.md) to commits. What makes the different than other references is their usage. They are used to mark specific points in your repo history.

These points are generally released versions.

The files for tags are stored in `.git/refs/tags/` directory.

There are two types of tags:

1. Lightweight tags
2. Annotated tags

## Lightweight Tags

They only store the reference to the commit and nothing else.

## Annotated Tags

These tags store metadata along with the reference.

This metadata includes:

- tagger-name
- tagger-email
- tag-timestamp
- tag-message
- and may be something more...

Metadata for these tags is stored as a tag-object in git object-store. The tag file in ref-store points to the hash of this object.

## References

- https://git-scm.com/book/en/v2/Git-Basics-Tagging