# Brace Expansion

**Syntax:** `{`comma-separted-values or sequence-expression`}`

A sequence-expression takes the form of `x..y` or `x..y..z` where `x` and `y` are either integers or characters and `z` is the step amount. Specifying `z` is optional.

**Example:**

```bash
a{d,c,b}e # -> ade ace abe
{0..10} # -> 0 1 2 3 4 5 6 7 8 9 10
{0..10..2} # -> 0 2 4 6 8 10
{00..10} # -> 00 01 02 03 04 05 06 07 08 09 10
{3..-4} # -> 3 2 1 0 -1 -2 -3 -4
{A..H} # -> A B C D E F G H
```

A correct brace expansion is expanded by an incorrectly formed brace expansion is left unchanged.

## References

- https://www.gnu.org/software/bash/manual/bash.html#Brace-Expansion
