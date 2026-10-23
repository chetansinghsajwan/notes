The module system is a Nix language library that enables you to
- Declare one attribute set using many separate Nix expressions.
- Imposes type constraints on values in that attribute set.
- Define values for the same attribute in different Nix expressions and merge these values automatically according to their type.
These Nix expressions are called modules and must have a particular structure.
---
The module system is a language for handling configuration, implemented as a Nix library.
Compared to plain Nix, it adds documentation, type checking and composition or extensibility.
From: https://nixos.org/manual/nixpkgs/stable/#module-system
---
# **Declaring options**
Options are declared under the top-level `options` attribute with `**[lib.mkOption](https://nixos.org/manual/nixpkgs/stable/#function-library-lib.options.mkOption)**`.
```nix
{ lib, ... }:
{
  options = {
    name = lib.mkOption { type = lib.types.str; };
  };
}
```
Option types: https://nixos.org/manual/nixos/stable/#sec-option-types
# References
- https://nix.dev/tutorials/module-system/index.html