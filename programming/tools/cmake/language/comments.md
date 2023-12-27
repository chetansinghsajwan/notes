# Comments

There are two types of comments.

## Line Comment

A `#` not immediately followed by a [`bracket_open`](https://cmake.org/cmake/help/latest/manual/cmake-language.7.html#grammar-token-bracket_open) forms a _line comment_ that runs until the end of the line.

### Syntax

```
line_comment ::=  '#' <any text not starting in a bracket_open
                       and not containing a newline>
```

> [!example]
> 
> ```cmake
> # This is a line comment.
> message("First Argument\n" # This is a line comment :)
>         "Second Argument") # This is a line comment.
> ```
  
## Bracket Comment

A `#` immediately followed by a [`bracket_open`](https://cmake.org/cmake/help/latest/manual/cmake-language.7.html#grammar-token-bracket_open) forms a _bracket comment_ consisting of the entire bracket enclosure.

### Syntax

```
bracket_comment ::=  '#' bracket_argument
```

> [!example]
> 
> ```cmake
> #[[This is a bracket comment.
> It runs until the close bracket.]]
> message("First Argument\n" #[[Bracket Comment]] "Second Argument")
> ```

## References

- https://cmake.org/cmake/help/latest/manual/cmake-language.7.html#id24