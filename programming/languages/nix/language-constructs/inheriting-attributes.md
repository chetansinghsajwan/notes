---
status: fetch-not-completed
---

# Inhertiting attributes

When defining an [attribute set](https://nixos.org/manual/nix/stable/language/values#attribute-set) or in a [let-expression](https://nixos.org/manual/nix/stable/language/constructs#let-expressions) it is often convenient to copy variables from the surrounding lexical scope (e.g., when you want to propagate attributes). This can be shortened using the `inherit` keyword.

---

###### Example

```nix
let x = 123; in
{
  inherit x;
  y = 456;
}
```

is equivalent to

```nix
let x = 123; in
{
  x = x;
  y = 456;
}
```

---

It is also possible to inherit attributes from another attribute set.

## References

- https://nixos.org/manual/nix/stable/language/constructs#inheriting-attributes