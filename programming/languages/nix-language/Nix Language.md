The Nix expression language is a pure, lazy, functional language.
The language was designed especially for the [Nix Package Manager](https://nixos.wiki/wiki/Nix_Package_Manager).
_domain-specific_
It comes with [built-in functions](https://nixos.org/manual/nix/stable/language/builtins) to integrate with the Nix store, which manages files and performs the derivations declared in the Nix language.
_declarative_
There is no notion of executing sequential steps.  
Dependencies between operations are established only through data.
_pure_
Values cannot change during computation.  
Functions always produce the same output if their input does not change.
_functional_
Functions are like any other value.  
Functions can be assigned to names, taken as arguments, or returned by functions.
_lazy_
Values are only computed when they are needed.
_dynamically typed_
Type errors are only detected when expressions are evaluated.
_nix-expression_: Every thing in nix-language is an expression.
  
[[Overview]]
[[Data Types]]
[[programming/languages/nix-language/Comments|Comments]]
# References

> [!info] Nix Language - Nix Reference Manual  
> The Nix language is designed for conveniently creating and composing derivations – precise descriptions of how contents of existing files are used to derive new files.  
> [https://nixos.org/manual/nix/stable/language/index.html](https://nixos.org/manual/nix/stable/language/index.html)