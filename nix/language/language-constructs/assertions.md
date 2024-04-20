# Assertions

```nix
assert e1; e2
```

where `e1` and `e2` are expressions.

If expression `e1` evaluates to `true`, `e2` is returned, otherwise evaluation is aborted and a backtrace is printed.

## References

- https://nixos.org/manual/nix/stable/language/constructs#assertions