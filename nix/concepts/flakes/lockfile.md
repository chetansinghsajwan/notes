# Flake Lock File

Inputs specified in `flake.nix` are typically "unlocked" in the sense that they don't specify an exact revision. To ensure reproducibility, Nix will automatically generate and use a _lock file_ called `flake.lock` in the flake's directory. The lock file contains a graph structure isomorphic to the graph of dependencies of the root flake. Each node in the graph (except the root node) maps the (usually) unlocked input specifications in `flake.nix` to locked input specifications. Each node also contains some metadata, such as the dependencies (outgoing edges) of the node.

For example, if `flake.nix` has the inputs in the example above, then the resulting lock file might be:

```json
{
  "version": 7,
  "root": "n1",
  "nodes": {
    "n1": {
      "inputs": {
        "nixpkgs": "n2",
        "import-cargo": "n3",
        "grcov": "n4"
      }
    },
    "n2": {
      "inputs": {},
      "locked": {
        "owner": "edolstra",
        "repo": "nixpkgs",
        "rev": "7f8d4b088e2df7fdb6b513bc2d6941f1d422a013",
        "type": "github",
        "lastModified": 1580555482,
        "narHash": "sha256-OnpEWzNxF/AU4KlqBXM2s5PWvfI5/BS6xQrPvkF5tO8="
      },
      "original": {
        "id": "nixpkgs",
        "type": "indirect"
      }
    },
    "n3": {
      "inputs": {},
      "locked": {
        "owner": "edolstra",
        "repo": "import-cargo",
        "rev": "8abf7b3a8cbe1c8a885391f826357a74d382a422",
        "type": "github",
        "lastModified": 1567183309,
        "narHash": "sha256-wIXWOpX9rRjK5NDsL6WzuuBJl2R0kUCnlpZUrASykSc="
      },
      "original": {
        "owner": "edolstra",
        "repo": "import-cargo",
        "type": "github"
      }
    },
    "n4": {
      "inputs": {},
      "locked": {
        "owner": "mozilla",
        "repo": "grcov",
        "rev": "989a84bb29e95e392589c4e73c29189fd69a1d4e",
        "type": "github",
        "lastModified": 1580729070,
        "narHash": "sha256-235uMxYlHxJ5y92EXZWAYEsEb6mm+b069GAd+BOIOxI="
      },
      "original": {
        "owner": "mozilla",
        "repo": "grcov",
        "type": "github"
      },
      "flake": false
    }
  }
}
```

This graph has 4 nodes: the root flake, and its 3 dependencies. The nodes have arbitrary labels (e.g. `n1`). The label of the root node of the graph is specified by the `root` attribute. Nodes contain the following fields:

- `inputs`: The dependencies of this node, as a mapping from input names (e.g. `nixpkgs`) to node labels (e.g. `n2`).
    
- `original`: The original input specification from `flake.lock`, as a set of `builtins.fetchTree` arguments.
    
- `locked`: The locked input specification, as a set of `builtins.fetchTree` arguments. Thus, in the example above, when we build this flake, the input `nixpkgs` is mapped to revision `7f8d4b088e2df7fdb6b513bc2d6941f1d422a013` of the `edolstra/nixpkgs` repository on GitHub.
    
    It also includes the attribute `narHash`, specifying the expected contents of the tree in the Nix store (as computed by `nix hash-path`), and may include input-type-specific attributes such as the `lastModified` or `revCount`. The main reason for these attributes is to allow flake inputs to be substituted from a binary cache: `narHash` allows the store path to be computed, while the other attributes are necessary because they provide information not stored in the store path.
    
- `flake`: A Boolean denoting whether this is a flake or non-flake dependency. Corresponds to the `flake` attribute in the `inputs` attribute in `flake.nix`.
    

The `original` and `locked` attributes are omitted for the root node. This is because we cannot record the commit hash or content hash of the root flake, since modifying `flake.lock` will invalidate these.

The graph representation of lock files allows circular dependencies between flakes. For example, here are two flakes that reference each other:

```nix
{
  inputs.b = ... location of flake B ...;
  # Tell the 'b' flake not to fetch 'a' again, to ensure its 'a' is
  # *this* 'a'.
  inputs.b.inputs.a.follows = "";
  outputs = { self, b }: {
    foo = 123 + b.bar;
    xyzzy = 1000;
  };
}
```

and

```nix
{
  inputs.a = ... location of flake A ...;
  inputs.a.inputs.b.follows = "";
  outputs = { self, a }: {
    bar = 456 + a.xyzzy;
  };
}
```

Lock files transitively lock direct as well as indirect dependencies. That is, if a lock file exists and is up to date, Nix will not look at the lock files of dependencies. However, lock file generation itself _does_ use the lock files of dependencies by default.

## References

- https://nixos.org/manual/nix/stable/command-ref/new-cli/nix3-flake#lock-files
