- A **flake** is a standardized, self-contained unit of Nix code with **explicit dependencies** and **reproducible outputs**.
- Introduced to fix problems with classic Nix: impure imports, unpinned dependencies, no standard structure.
- Defined in a file called `flake.nix` + auto-generated `flake.lock`.
## Two Core Parts
|Part|Purpose|
|---|---|
|`inputs`|Declares dependencies (other flakes/repos) — pinned via `flake.lock`|
|`outputs`|A function that takes inputs and produces things (packages, configs, apps, etc.)|
## Key Concepts
- **Purity**: Flakes evaluate in a sandboxed, hermetic way — no reading `NIX_PATH`, no ambient system state. Same input = same output, always.
- **Lock file (**`**flake.lock**`**)**: Pins exact commit/hash of every input (direct + transitive). Guarantees reproducibility across machines/time.
- `**follows**`: Forces one input's dependency to reuse another input's version (avoids duplicate/conflicting nixpkgs versions).
## Common Output Types
|Output|Used By|Example|
|---|---|---|
|`packages`|`nix build`|Custom package builds|
|`devShells`|`nix develop`|Dev environments|
|`apps`|`nix run`|Runnable programs|
|`nixosConfigurations`|`nixos-rebuild`|Full NixOS systems|
|`darwinConfigurations`|`darwin-rebuild`|macOS systems|
|`homeConfigurations`|`home-manager`|Standalone user configs|
|`overlays`|Other flakes|Package modifications|
|`lib`|Other flakes|Shared helper functions|
## Why Flakes (vs classic Nix)
- ✅ Reproducible builds (locked versions)
- ✅ No global `NIX_PATH`/channels needed
- ✅ Composable — flakes can consume other flakes cleanly
- ✅ Standard CLI (`nix build`, `nix run`, `nix flake show`, etc.)
- ⚠️ Still officially "experimental" (but widely used in production)
## Your Flake, Mapped
- `inputs`: nixpkgs, nur, nix-darwin, home-manager, vscode-extensions
- `outputs`: `nixosConfigurations.nixos` + `darwinConfigurations.macbook-air-m3`
- `flake.lock`: pins exact commits of all 5 inputs → same system builds on any machine, anytime.