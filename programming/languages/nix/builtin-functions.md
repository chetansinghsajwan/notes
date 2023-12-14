# Builtin Functions

^main

All built-in functions are available through the global [`builtins`](builtin-constants#^builtins) constant.

For convenience, some built-ins can be accessed directly:

- [`derivation`](builtin-functions/derivation)
- [`import`](builtin-functions/import)
- [`abort`](builtin-functions/abort)
- [`throw`](builtin-functions/throw)

### Core

> ##### `typeOf e`
> 
> Return a string representing the type of the value e, namely "int", "bool", "string", "path", "null", "set", "list", "lambda" or "float".
^type-of

> ##### `throw <msg>`
> 
> Throw an error message `msg`.
^throw

> ##### `break v` ^break
> 
> In debug mode (enabled using `--debugger`), pause Nix expression evaluation and enter the REPL. Otherwise, return the argument `v`.
^break

> ##### `import` `path`
> 
> Load, parse, and return the Nix expression in the file `path`.
> 
> The `path` argument must meet the same criteria as an [interpolated expression](https://nixos.org/manual/nix/stable/language/string-interpolation#interpolated-expression).
> 
> If `path` is a directory, the file `default.nix` in that directory is used if it exists.
> 
> Evaluation aborts if the file doesn’t exist or contains an invalid Nix expression.
^import

> ##### `derivation` `attrs`
> 
> 
^derivation

### Math Operations
^math-functions

> ##### `add <e1> <e2>`
> 
> Return the sum of the numbers `<e1>` and `<e2>`.
^add

> ##### `sub <e1> <e2>`
> 
> Return the difference between the numbers `<e1>` and `<e2>`.
^sub

> ##### `mul <e1> <e2>`
> 
> Return the product of the numbers `<e1>` and `<e2>`.
^mul

> ##### `div <e1> <e2>`
> 
> Return the quotient of the numbers `<e1>` and `<e2>`.
^div

> ##### `bitAnd <e1> <e2>``
> 
> Return the bitwise AND of the integers `<e1>` and `<e2>`.
^bit-and

> ##### `bitOr <e1> <e2>``
> 
> Return the bitwise OR of the integers `<e1>` and `<e2>`.
^bit-or

> ##### `bitXor <e1> <e2>``
> 
> Return the bitwise XOR of the integers `<e1>` and `<e2>`.
^bit-xor

> ##### `ceil <n>`
> 
> Converts an IEEE-754 double-precision floating-point number `<n>` to the next higher integer.
> 
> If the datatype is neither an integer nor a float, an evaluation error will be thrown.
^ceil

> ##### `floor double`
> 
> Converts an IEEE-754 double-precision floating-point number (_double_) to the next lower integer.
> 
> If the datatype is neither an integer nor a "float", an evaluation error will be thrown.
^floor

### Comparisions
^comparision-functions

> ##### `all <pred> <list>`
> 
> Return `true` if the function `<pred>` returns `true` for all elements of `list`, and `false` otherwise.
^all

> ##### `any <pred> <list>`
> 
> Return `true` if the function `<pred>` returns `true` for all elements of `list`, and `false` otherwise.
^all

> ##### `isAttrs e`
> 
> Return `true` if _e_ evaluates to a set, and `false` otherwise.
^is-attrs

> ##### `isBool e`
> 
> Return `true` if _e_ evaluates to a bool, and `false` otherwise.
^is-bool

> ##### `isFloat e`
> 
> Return `true` if _e_ evaluates to a float, and `false` otherwise.
^is-float

> ##### `isFunction e`
> 
> Return `true` if _e_ evaluates to a function, and `false` otherwise.
^is-function

> ##### `isInt e`
> 
> Return `true` if _e_ evaluates to an integer, and `false` otherwise.
^is-int

> ##### `isList e`
> 
> Return `true` if _e_ evaluates to a list, and `false` otherwise.
^is-list

> ##### `isPath e`
> 
> Return `true` if _e_ evaluates to a path, and `false` otherwise.
^is-path

> ##### `isString e`
> 
> Return `true` if _e_ evaluates to a string, and `false` otherwise.
^is-string

### Conversions

> ##### `toFile name s`
> 
> Store the string _s_ in a file in the Nix store and return its path as `string`.
> 
> The file has suffix _name_.
^to-file

> ##### `toJSON e`
> 
> Return a string containing a JSON representation of _e_. Strings, integers, floats, booleans, nulls and lists are mapped to their JSON equivalents. Sets (except derivations) are represented as objects. Derivations are translated to a JSON string containing the derivation’s output path. Paths are copied to the store and represented as a JSON string of the resulting store path.
^to-json

> ##### `toString e`
> 
> 
^to-string

> ##### `toXML e`
> 
> 
^to-xml


### File Operations
^file-op-functions

> ##### `readFile <path>`
> 
> Return the contents of the file `<path>` as a `string`.
^read-file

> ##### `pathExists <path>`
> 
> Return `true` if the path `<path>` exists at evaluation time, and `false` otherwise.
^path-exists

### Unsorted Functions
^unsorted-functions

> ##### `length <list>`
> 
> Return the length of the list `<list>`.
^length

> ##### `lessThan <e1> <e2>`
> 
> Return `true` if the number `<e1>` is less than the number `<e2>`, and `false` otherwise.
> 
> Evaluation aborts if either `<e1>` or `<e2>` does not evaluate to a number.
^less-than


## References

^references

> https://nixos.org/manual/nix/stable/language/builtins