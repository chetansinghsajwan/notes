# `attribute-set` type

An attribute set is a collection of name-value-pairs (called **attributes**) enclosed in curly brackets (`{ }`).

An attribute name can be an identifier or a string.

An identifier must start with a letter (`a-z`, `A-Z`) or underscore (`_`), and can otherwise contain letters (`a-z`, `A-Z`), numbers (`0-9`), underscores (`_`), apostrophes (`'`), or dashes (`-`).

---

###### Example

Creates an attribute set of two variables, `b` contains default value.

```nix
{
	a;
	b = "Bar";
}
```

---
###### Related

- [attribute-selection](attribute-selection.md)
- [recursive-set](recursive-set.md)

---

## References

- https://nixos.org/manual/nix/stable/language/values#attribute-set