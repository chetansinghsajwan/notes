# `list` type

Lists are formed by enclosing a whitespace separated list of values between square brackets.

Note that lists are only lazy in values, and they are strict in length.

> [!example]
> 
> Defines a list of four elements, the last being the result of a call to the function `f`.
> 
> ```nix
> [ 123 ./foo.nix "abc" (f { x = y; }) ]
> ```