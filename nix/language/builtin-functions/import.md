# `import` function

```
import <path>
```

Load, parse, and return the Nix expression in the file `path`.

The `path` argument must meet the same criteria as an [interpolated expression](https://nixos.org/manual/nix/stable/language/string-interpolation#interpolated-expression).

If `path` is a directory, the file `default.nix` in that directory is used if it exists.

Evaluation aborts if the file doesn’t exist or contains an invalid Nix expression.

## References

- https://nixos.org/manual/nix/stable/language/builtins#builtins-import