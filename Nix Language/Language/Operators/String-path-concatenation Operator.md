syntax:: `<string> + <path>`  
associativity:: left  
precedence:: 7
Concatenate [[String Type]] with [[Path Type]]. The result is a string.

> [!note]
> 
> The file or directory at `path` must exist and is copied to the [store](https://nixos.org/manual/nix/stable/glossary#gloss-store). The path appears in the result as the corresponding [store path](https://nixos.org/manual/nix/stable/glossary#gloss-store-path).
## Related
- [[Path-string-concatenation Operator]]
## References
- https://nixos.org/manual/nix/stable/language/operators\#string-and-path-concatenation