# With Expressions

A with-expression,

```nix
with e1; e2
```

introduces the set `e1` into the lexical scope of the expression `e2`.

- **Example**
  
  ```nix
  let as = { x = "foo"; y = "bar"; };
  in with as; x + y
  ```
  
  evaluates to `"foobar"` since the `with` adds the `x` and `y` attributes of `as` to the lexical scope in the expression `x + y`.

The most common use of `with` is in conjunction with the `import` function.

> [!example]
> 
>  ```nix
> with (import ./definitions.nix); ...
> ```
> 
> makes all attributes defined in the file `definitions.nix` available as if they were defined locally in a `let`-expression.

The bindings introduced by `with` do not shadow bindings introduced by other means, e.g.

```nix
let a = 3; in with { a = 1; }; let a = 4; in with { a = 2; }; ...
```

establishes the same scope as

```nix
let a = 1; in let a = 2; in let a = 3; in let a = 4; in ...
```

## References

- https://nixos.org/manual/nix/stable/language/constructs#with-expressions