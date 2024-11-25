# Inhertiting attributes

```
inherit <attr>;
inherit (<attr-set>) <attr>;
```

`inherit` keyword allows you to copy variables from the surronding scope into this scope.

The second syntax does the same, it just allows to pass the attribute set `attr-set` whose variables you want to copy.

---

###### Example

This brings `x` from the surrounding scope created by `let` into the scope of the attribute set.

```nix
let x = 123; in
{
  inherit x;
  y = 456;
}
```

The above is equivalent to:

```nix
let x = 123; in
{
  x = x;
  y = 456;
}
```

---

## References

- https://nixos.org/manual/nix/stable/language/constructs#inheriting-attributes