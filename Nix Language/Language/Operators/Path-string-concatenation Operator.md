syntax:: `<path> + <string>`  
associativity:: left  
precedence:: 7
Concatenate [[Path Type]] with [[String Type]]. The result is a path.

> [!note]
> 
> The string must not have a string context that refers to a [store path](https://nixos.org/manual/nix/stable/glossary#gloss-store-path).

> [!todo]
> 
> Explain the above statement.
## Related
- [[String-path-concatenation Operator]]
## References
- https://nixos.org/manual/nix/stable/language/operators\#path-and-string-concatenation