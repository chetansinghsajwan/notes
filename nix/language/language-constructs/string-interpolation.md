# String Interpolation

String interpolation is a language feature where a [string](nix/language/data-types/string.md), [path](path.md), or [attribute name](set.md) can contain expressions enclosed in `${ }`. Such a construct is called *interpolated string*, and the expression inside is an [interpolated expression](#^interpolated-expression).

## Interpolated Expression
^interpolated-expression

An expression that is interpolated must evaluate to one of the following:

- [string](nix/language/data-types/string.md)
  
  A string interpolates to itself.

- [path](path.md)
  
  A path in an interpolated expression is first copied into the Nix store, and the resulting string is the [store path](https://nixos.org/manual/nix/stable/glossary#gloss-store-path) of the newly created [store object](https://nixos.org/manual/nix/stable/glossary#gloss-store-object).
  
  > [!example]
  > 
  > ```shell-instance
  > $ mkdir foo
  > ```
  > 
  > Reference the empty directory in an interpolated expression:
  > 
  > ```nix
  > "${./foo}"
  > 
  > # will result in 
  > 
  > "/nix/store/2hhl2nz5v0khbn06ys82nrk99aa1xxdw-foo"
  > ```

- [derivation](programming/languges/nix/derivation)
  
  A derivation interpolates to the [store path](https://nixos.org/manual/nix/stable/glossary#gloss-store-path) of its first [output](https://nixos.org/manual/nix/stable/language/derivations#attr-outputs).
  
  > [!example]
  > 
  > ```nix
  > let
  >   pkgs = import <nixpkgs> {};
  > in
  > "${pkgs.hello}"
  > ```
  > 
  > ```
  > "/nix/store/4xpfqf29z4m8vbhrqcz064wfmb46w5r7-hello-2.12.1"
  > ```

- [attribute set](set.md)
    
    - If the attribute-set contains `__toString` attribute. `__toString` must be a function that takes the attribute set itself and returns a string.
      
      The attribute-set is interpolated to the return value of the funciton.
      
      > [!example]
      > 
      > ```nix
      > let
      >   a = {
      >     value = 1;
      >     __toString = self: toString (self.value + 1);
      >   };
      > in
      > "${a}"
      > ```
      > 
      > evalutes to 
      > 
      > ```nix
      > "2"
      > ```

    - Else if the attribute-set contains attribute `outPath`, The set is interpolated to its value.
      
      > [!example]
      > 
      > ```nix
      > let
      >   a = { outPath = "foo"; };
      > in
      > "${a}"
      > ```
      > 
      > evalutes to
      > 
      > ```nix
      > "foo"
      > ```

    - Else an error is thrown.
      
      > [!example]
      > 
      > ```nix
      > let
      >   a = {};
      > in
      >   "${a}"
      > ```
      > 
      > ```
      > error: cannot coerce a set to a string
      > 
      >        at «string»:4:2:
      > 
      >             3| in
      >             4| "${a}"
      >              |  ^
      > ```

## References

- https://nixos.org/manual/nix/stable/language/string-interpolation