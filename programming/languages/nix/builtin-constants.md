# Builtin Constants

^main

These constants are built into the Nix language evaluator.

> ##### `builtins` (set)
> 
> Contains all the [built-in functions](builtin-functions) and values.
> 
> > ##### Example
> > 
> > ```nix
> > if builtins.hasContext s
> > ```
> 
> ^builtins

> ##### `true` (bool)
> 
> Primitive value.
> 
> ^true

> ##### `false` (bool)
> 
> Primitive value.
> 
> ^false

> ##### `null` (null)
> 
> Primitive value.
> 
> ^null

> ##### `langVersion` (integer)
> 
> The current version of the Nix language.
> 
> ^lang-version

> ##### `currentSystem` (string)
> 
> The value of the [`system` configuration option](https://nixos.org/manual/nix/stable/command-ref/conf-file#conf-pure-eval).
> 
> ^current-system

> ##### `currentTime` (integer)
> 
> Returns the [Unix time](dictionary/unix-time) at first evaluation. Repeated references to that name will re-use the initially obtained value.
> 
> ^current-time

> ##### `storeDir` (string)
> 
> Logical file system location of the [Nix store](unknown) currently in use.
> 
> ^store-dir

> ##### `nixPath` (list)
> 
> List of search path entries used to resolve [lookup paths](programming/languages/nix/language-constructs/lookup-paths).
> 
> ^nix-path

> ##### `nixVersion` (string)
> 
> The version of Nix.
> 
> > ##### Example
> > 
> > ```nix-repl
> > nix-repl> builtins.nixVersion
> > "2.16.0"
> > ```
> 
> ^nix-version

## References

^references

> https://nixos.org/manual/nix/stable/language/builtin-constants
