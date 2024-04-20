# Flake References

Flake references (_flakerefs_) are a way to specify the location of a flake. These have two different forms:

## Attribute set representation

Example:

```nix
{
  type = "github";
  owner = "NixOS";
  repo = "nixpkgs";
}
```

The only required attribute is `type`. The supported types are listed below.

### URL-like syntax

Example:

```
github:NixOS/nixpkgs
```

These are used on the command line as a more convenient alternative to the attribute set representation. For instance, in the command

```console
 nix build github:NixOS/nixpkgs#hello
```

`github:NixOS/nixpkgs` is a flake reference (while `hello` is an output attribute). They are also allowed in the `inputs` attribute of a flake, e.g.

```nix
inputs.nixpkgs.url = "github:NixOS/nixpkgs";
```

is equivalent to

```nix
inputs.nixpkgs = {
    type = "github";
    owner = "NixOS";
    repo = "nixpkgs";
};
```

## References

- https://nixos.org/manual/nix/stable/command-ref/new-cli/nix3-flake#flake-references
