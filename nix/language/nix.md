# Nix Language

The Nix expression language is a pure, lazy, functional language.

The language was designed especially for the [nix](nix/nix.md) package manger.

###### domain-specific

It comes with [built-in functions](builtin-functions.md) to integrate with the Nix store, which manages files and performs the derivations declared in the Nix language.

###### declarative

There is no notion of executing sequential steps.  
Dependencies between operations are established only through data.

###### pure

Values cannot change during computation.  

Functions always produce the same output if their input does not change.

###### functional

Functions are like any other value.  

Functions can be assigned to names, taken as arguments, or returned by functions.

###### lazy

Values are only computed when they are needed.

###### dynamically-typed

Type errors are only detected when expressions are evaluated.

## References

- https://nixos.org/manual/nix/stable/language/index.html
