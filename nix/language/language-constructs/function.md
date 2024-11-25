# Function

A function has the following form:

```
pattern: body
```

The pattern specifies what the argument of the function must look like, and binds variables in the body to (parts of) the argument.

There are three kinds of patterns:

- If a pattern is a single identifier, then the function matches any argument.

  > [!example]
  > 
  > ```nix
  > let negate = x: !x;
  >     concat = x: y: x + y;
  > in if negate true then concat "foo" "bar" else ""
  > ```
  > 
  > Note that `concat` is a function that takes one argument and returns a function that takes another argument.

- A set pattern of the form `{ name1, name2, …, nameN }` matches a set containing exactly (no more, no less) the listed attributes, and binds the values of those attributes to variables in the function body.
  
  > [!example]
  > 
  > ```nix
  > { x, y, z }: z + y + x
  > ```

  To allow additional arguments, you can use an ellipsis (`...`).

  > [!example]
  > 
  > ```nix
  > { x, y, z, ... }: z + y + x
  > ```
  > 
  > This works on any set that contains at least the three named attributes. It is possible to provide default values for attributes, in which case they are allowed to be missing. A default value is specified by writing `name ? e`, where `e` is an arbitrary expression.

  > [!example]
  > 
  > ```nix
  > { x, y ? "foo", z ? "bar" }: z + y + x
  > ```
  > 
  > Specifies a function that only requires an attribute named `x`, but optionally accepts `y` and `z`.

- An `@`-pattern provides a means of referring to the whole value being matched.

  ```nix
  args@{ x, y, z, ... }: z + y + x + args.a
  ```
  
  but can also be written as:

  ```nix
  { x, y, z, ... } @ args: z + y + x + args.a
  ```

  Here args is bound to the argument as passed, which is further matched against the pattern { x, y, z, ... }.

  > [!tip]
  > 
  > The @-pattern makes mainly sense with an ellipsis(...) as you can access attribute names as a, using args.a, which was given as an additional attribute to the function.

## References

- https://nixos.org/manual/nix/stable/language/constructs#functions