# Sandboxing

Whenever Nix builds anything, it sandboxes that process from everything else on the host system.

Nix builds are sandboxed for a variety of reasons:

1. The ensure [reproducibility](https://zero-to-nix.com/concepts/reproducibility). Sandboxing ensures that no system state on the host machine affects the build outcomes.

2. To maintain strict [provenance](https://zero-to-nix.com/concepts/provenance).

## References

- https://zero-to-nix.com/concepts/sandboxing