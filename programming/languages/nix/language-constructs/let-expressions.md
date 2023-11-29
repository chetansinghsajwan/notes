# Let Expressions
^main

A let-expression allows you to define local variables for an expression.

> [!syntax]
> 
> ```nix
> <let-in> = let [ <identifier> = <expr> ]... in <expr>
> ```

> [!example]
> 
> ```nix
> let
>   x = "foo";
>   y = "bar";
> in x + y
> ```
> 
> This evaluates to `"foobar"`.

## References
^references

> https://nixos.org/manual/nix/stable/language/constructs#let-expressions