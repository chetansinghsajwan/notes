# `path` type

A path must contain at least one slash to be recognized as such.

For instance, `builder.sh` is not a path: it's parsed as an expression that selects the attribute `sh` from the variable `builder`.

If the file name is relative, i.e., if it does not begin with a slash, it is made absolute at parse time relative to the directory of the Nix expression that contained it. For instance, if a Nix expression in `/foo/bar/bla.nix` refers to `../xyzzy/fnord.nix`, the absolute path is `/foo/xyzzy/fnord.nix`.

If the first component of a path is a `~`, it is interpreted as if the rest of the path were relative to the user's home directory. e.g. `~/foo` would be equivalent to `/home/edolstra/foo` for a user whose home directory is `/home/edolstra`.

Paths can also be specified between angle brackets, e.g. `<nixpkgs>`. This means that the directories listed in the environment variable `NIX_PATH` will be searched for the given file or directory name.

When an [interpolated string](https://nixos.org/manual/nix/stable/language/string-interpolation) evaluates to a path, the path is first copied into the Nix store and the resulting string is the [store path](https://nixos.org/manual/nix/stable/glossary#gloss-store-path) of the newly created [store object](https://nixos.org/manual/nix/stable/glossary#gloss-store-object).

For instance, evaluating `"${./foo.txt}"` will cause `foo.txt` in the current directory to be copied into the Nix store and result in the string `"/nix/store/<hash>-foo.txt"`.

Paths themselves, except those in angle brackets (`< >`), support [string interpolation](https://nixos.org/manual/nix/stable/language/string-interpolation).

At least one slash (`/`) must appear _before_ any interpolated expression for the result to be recognized as a path.

## References

- https://nixos.org/manual/nix/stable/language/values#type-path