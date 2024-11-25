# `nix flake new` command

## Name

Creates a flake in the specified directory from a template

## Synopsis

```
nix flake new <dest-dir> [option...]
```

## Examples

- Create a flake using the default template in the directory `hello`:
    

- ```console
     nix flake new hello
    ```
    
- List available templates:
    
- ```console
     nix flake show templates
    ```
    
- Create a flake from a specific template in the directory `hello`:
    

```console
nix flake new hello -t templates#trivial
```

## Description

This command creates a flake in the directory `dest-dir`, which must not already exist. It's equivalent to:

```console
mkdir dest-dir
cd dest-dir
nix flake init
```

## Options

- `--template`,`-t` template
    
    The template to use.

## References

- https://nixos.org/manual/nix/stable/command-ref/new-cli/nix3-flake-new
