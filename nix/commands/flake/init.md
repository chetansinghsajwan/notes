# `nix flake init` command

## Name

Creates a flake in the current directory from a template.

## Synopsis

```
nix flake init [option...]
```

## Examples

- Create a flake using the default template:
    

- ```console
     nix flake init
    ```
    
- List available templates:
    
- ```console
     nix flake show templates
    ```
    
- Create a flake from a specific template:
    

- ```console
     nix flake init -t templates#simpleContainer
    ```
    

# [Description](https://nixos.org/manual/nix/stable/command-ref/new-cli/nix3-flake-init#description)

This command creates a flake in the current directory by copying the files of a template. It will not overwrite existing files. The default template is `templates#templates.default`, but this can be overridden using `-t`.

# [Template definitions](https://nixos.org/manual/nix/stable/command-ref/new-cli/nix3-flake-init#template-definitions)

A flake can declare templates through its `templates` output attribute. A template has the following attributes:

- `description`: A one-line description of the template, in CommonMark syntax.

- `path`: The path of the directory to be copied.

- `welcomeText`: A block of markdown text to display when a user initializes a new flake based on this template.


Here is an example:

```nix
outputs = { self }: {

  templates.rust = {
    path = ./rust;
    description = "A simple Rust/Cargo project";
    welcomeText = ''
      # Simple Rust/Cargo Template
      ## Intended usage
      The intended usage of this flake is...

      ## More info
      - [Rust language](https://www.rust-lang.org/)
      - [Rust on the NixOS Wiki](https://nixos.wiki/wiki/Rust)
      - ...
    '';
  };

  templates.default = self.templates.rust;
}
```

## Options

- `--template`, `-t` _template_
    
    The template to use.

## References

- https://nixos.org/manual/nix/stable/command-ref/new-cli/nix3-flake-init
