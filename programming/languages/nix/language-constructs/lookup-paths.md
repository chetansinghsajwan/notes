# Lookup Paths

> [!syntax]
> 
> _lookup-path_ = `<` _identifier_ [ `/` _identifier_ ]... `>`

A lookup path expression is just a syntatic sugar and can be [desugared](https://en.wikipedia.org/wiki/Syntactic_sugar) using [`builtins.findFile`](programming/languages/nix/builtin-functions/find-file) and [`builtins.nixPath`](programming/languages/nix/builtin-constants/nix-path):

```nix
<nixpkgs>
```

is equivalent to:

```nix
builtins.findFile builtins.nixPath "nixpkgs"
```

## References

- https://nixos.org/manual/nix/stable/language/constructs/lookup-path