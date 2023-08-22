The Nix language is designed for conveniently creating and composing _derivations_ – precise descriptions of how contents of existing files are used to derive new files. It is:

- _domain-specific_
    
    It comes with [built-in functions](https://nixos.org/manual/nix/stable/language/builtins) to integrate with the Nix store, which manages files and performs the derivations declared in the Nix language.
    
- _declarative_
    
    There is no notion of executing sequential steps. Dependencies between operations are established only through data.
    
- _pure_
    
    Values cannot change during computation. Functions always produce the same output if their input does not change.
    
- _functional_
    
    Functions are like any other value. Functions can be assigned to names, taken as arguments, or returned by functions.
    
- _lazy_
    
    Values are only computed when they are needed.
    
- _dynamically typed_
    
    Type errors are only detected when expressions are evaluated.
    

# References

* https://nixos.org/manual/nix/stable/language/