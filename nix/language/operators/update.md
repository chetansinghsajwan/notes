# `update` operator

syntax:: `<set1> // <set2>`
associativity:: right
precedence:: 9

Update [attribute set](programming/languages/nix/data-types/attribute-set) `set1` with names and values from `set2`.

The returned attribute set will have of all the attributes in `set1` and `set2`.

If an attribute name is present in both, the attribute value from the `set2` is taken.

## References

- https://nixos.org/manual/nix/stable/language/operators#update