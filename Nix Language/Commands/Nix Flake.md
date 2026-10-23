## Name
`nix flake`: manage nix flakes.
## Synopsis
```nix
`nix flake` [_option_...] _subcommand_
```
where _subcommand_ is one of the following:
- `nix flake archive` - copy a flake and all its inputs to a store
- `nix flake check` - check whether the flake evaluates and run its tests
- `nix flake clone` - clone flake repository
- `[[Nix Flake Info Command|nix flake info]]` - show flake metadata
- `[[Init|nix flake init]]` - create a flake in the current directory from a template
- `nix flake lock` - create missing lock file entries
- `[[Nix Flake Metadata Command|nix flake metadata]]` - show flake metadata
- `[[Nix Flake New Command|nix flake new]]` - create a flake in the specified directory from a template
- `nix flake prefetch` - download the source tree denoted by a flake reference into the Nix store
- `[[Nix Flake Show Command|nix flake show]]` - show the outputs provided by a flake
- `[[Nix Flake Update Command|nix flake update]]` - update flake lock file
## References
- https://nixos.org/manual/nix/stable/command-ref/new-cli/nix3-flake