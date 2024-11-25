# `string-path-concatenation` operator

syntax:: `<string> + <path>`
associativity:: left
precedence:: 7

Concatenate [string](nix/language/data-types/string.md) with [path](path.md). The result is a string.

> [!note]
> 
> The file or directory at `path` must exist and is copied to the [store](https://nixos.org/manual/nix/stable/glossary#gloss-store). The path appears in the result as the corresponding [store path](https://nixos.org/manual/nix/stable/glossary#gloss-store-path).

## Related

- [[path-string-concatenation]]

## References

- https://nixos.org/manual/nix/stable/language/operators#string-and-path-concatenation