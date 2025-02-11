# Recursive Sets

Recursive sets are like normal [attribute sets](nix/language/data-types.md), but the attributes can refer to each other.

```
<rec-attrset> =  rec { [ <name> = <expr>; ]... }
```

> [!example]
> 
> ```nix
> rec {
>   x = y;
>   y = 123;
> }.x
> ```
> 
> This evaluates to `123`.

> [!note]
> 
> Recursive sets of course introduce the danger of infinite recursion. For example, the expression
> 
> ```nix
> rec {
>   x = y;
>   y = x;
> }.x
> ```
> 
> will crash with an `infinite recursion encountered` error message.
> 

## References

- https://nixos.org/manual/nix/stable/language/constructs#recursive-sets