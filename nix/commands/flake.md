# `nix flake`

## Name

`nix flake`: manage nix flakes.

## Synopsis

```nix
`nix flake` [_option_...] _subcommand_
```

where _subcommand_ is one of the following:

- [`nix flake archive`](nix/commands/flake/archive) - copy a flake and all its inputs to a store
- [`nix flake check`](nix/commands/flake/check) - check whether the flake evaluates and run its tests
- [`nix flake clone`](nix/commands/flake/clone) - clone flake repository
- [`nix flake info`](nix/commands/flake/info) - show flake metadata
- [`nix flake init`](nix/commands/flake/init) - create a flake in the current directory from a template
- [`nix flake lock`](nix/commands/flake/lock) - create missing lock file entries
- [`nix flake metadata`](nix/commands/flake/metadata) - show flake metadata
- [`nix flake new`](nix/commands/flake/new) - create a flake in the specified directory from a template
- [`nix flake prefetch`](nix/commands/flake/prefetch) - download the source tree denoted by a flake reference into the Nix store
- [`nix flake show`](nix/commands/flake/show) - show the outputs provided by a flake
- [`nix flake update`](nix/commands/flake/update) - update flake lock file

## References

- https://nixos.org/manual/nix/stable/command-ref/new-cli/nix3-flake
