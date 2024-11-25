---
status: fetching-info
---

# Derivation

A _derivation_ is an instruction that Nix uses to [realise](https://zero-to-nix.com/concepts/realisation) a Nix package.

> [!tip] Mental model
> 
> You may find it helpful to think of a derivation as the plan or blueprint for a Nix package.

## Creating a derivation

They're created using a special [`derivation`](nix/language/builtin-functions/derivation.md) function in the nix-language.

While you can create derivations using the raw `derivation` function, it's far more common to use a wrapper function around it.

The most commonly used wrapper function is [`stdenv.mkDerivation`](https://nixos.org/manual/nixpkgs/stable/#sec-using-stdenv).

There are also other custom derivation wrappers for specific languages, frameworks, and more (and many of those actually wrap `stdenv.mkDerivation`).

For example,

- [`buildGoModule`](https://nixos.org/manual/nixpkgs/stable/#sec-language-go) for [go language](golang/golang)
- [`buildRustPackage`](https://nixos.org/manual/nixpkgs/stable/#rust) for [rust language](rust/rust)
- [`buildPythonApplication`](https://nixos.org/manual/nixpkgs/stable/#python) for [python language](python/python)

## References

- https://zero-to-nix.com/concepts/derivations
