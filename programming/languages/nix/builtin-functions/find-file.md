# `findFile` function

Find _lookup-path_ in _search-path_.

A search path is represented list of [attribute sets](https://nixos.org/manual/nix/stable/language/values#attribute-set) with two attributes:

- `prefix` is a relative path.
- `path` denotes a file system location The exact syntax depends on the command line interface.

Examples of search path attribute sets:

- `{   prefix = "nixos-config";   path = "/etc/nixos/configuration.nix"; }`
    
- `{   prefix = "";   path = "/nix/var/nix/profiles/per-user/root/channels"; }`
    

The lookup algorithm checks each entry until a match is found, returning a [path value](https://nixos.org/manual/nix/stable/language/values#type-path) of the match:

- If _lookup-path_ matches `prefix`, then the remainder of _lookup-path_ (the "suffix") is searched for within the directory denoted by `path`. Note that the `path` may need to be downloaded at this point to look inside.

- If the suffix is found inside that directory, then the entry is a match. The combined absolute path of the directory (now downloaded if need be) and the suffix is returned.

## References

- https://nixos.org/manual/nix/stable/language/builtins#builtins-findFile