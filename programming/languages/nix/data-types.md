# Data Types

#### Primitive Types

> ##### Number
> 
> Numbers, which can be _integers_ (like `123`) or _floating point_ (like `123.43` or `.27e13`).

> ##### Boolean
> 
> _Booleans_ with values `true` and `false`.

> ##### Null
> 
> The null value, denoted as `null`.

> #review 
> 
> ##### Path
> 
> 
> A path must contain at least one slash to be recognized as such.
> 
> For instance, `builder.sh` is not a path: it's parsed as an expression that selects the attribute `sh` from the variable `builder`.
> 
> If the file name is relative, i.e., if it does not begin with a slash, it is made absolute at parse time relative to the directory of the Nix expression that contained it. For instance, if a Nix expression in `/foo/bar/bla.nix` refers to `../xyzzy/fnord.nix`, the absolute path is `/foo/xyzzy/fnord.nix`.
> 
> If the first component of a path is a `~`, it is interpreted as if the rest of the path were relative to the user's home directory. e.g. `~/foo` would be equivalent to `/home/edolstra/foo` for a user whose home directory is `/home/edolstra`.
> 
> Paths can also be specified between angle brackets, e.g. `<nixpkgs>`. This means that the directories listed in the environment variable `NIX_PATH` will be searched for the given file or directory name.
> 
> When an [interpolated string](https://nixos.org/manual/nix/stable/language/string-interpolation) evaluates to a path, the path is first copied into the Nix store and the resulting string is the [store path](https://nixos.org/manual/nix/stable/glossary#gloss-store-path) of the newly created [store object](https://nixos.org/manual/nix/stable/glossary#gloss-store-object).
> 
> For instance, evaluating `"${./foo.txt}"` will cause `foo.txt` in the current directory to be copied into the Nix store and result in the string `"/nix/store/<hash>-foo.txt"`.
> 
> Paths themselves, except those in angle brackets (`< >`), support [string interpolation](https://nixos.org/manual/nix/stable/language/string-interpolation).
> 
> At least one slash (`/`) must appear _before_ any interpolated expression for the result to be recognized as a path.

> ##### String
> 
> Strings can be written in three ways.
> 
> 1. The most common way is to enclose the string between double quotes, e.g., `"foo bar"`.
> 
>    Strings can span multiple lines.
> 
>    The special characters `"` and `\` and the character sequence `${` must be escaped by prefixing them with a backslash (`\`).
> 
>    Newlines, carriage returns and tabs can be written as `\n`, `\r` and `\t`, respectively.
> 
>    You can include the results of other expressions into a string by enclosing them in `${ }`, a feature known as [string interpolation](programming/languages/nix/string-interpolation).
> 
> 2. The second way to write string literals is as an _indented string_, which is enclosed between pairs of _double single-quotes_, like so:
> 
>    ```nix
>    ''
> 	   This is the first line.
> 	   This is the second line.
> 	   This is the third line.
>    ''
>    ```
> 
> 3. Finally, as a convenience, [URIs](computer-science/uri) can be written as is, without quotes.
> 
>    > ##### Example
>    > 
>    > The string `"http://example.org/foo.tar.bz2"` can also be written as just `http://example.org/foo.tar.bz2` without any quotes.

#### Compound Types

> ##### List
> 
> Lists are formed by enclosing a whitespace separated list of values between square brackets.
> 
> Note that lists are only lazy in values, and they are strict in length.
> 
> > ##### Example
> > 
> > Defines a list of four elements, the last being the result of a call to the function `f`.
> > 
> > ```nix
> > [ 123 ./foo.nix "abc" (f { x = y; }) ]
> > ```

> ##### Attribute Set
> 
> An attribute set is a collection of name-value-pairs (called **attributes**) enclosed in curly brackets (`{ }`).
> 
> An attribute name can be an identifier or a string.
> 
> An identifier must start with a letter (`a-z`, `A-Z`) or underscore (`_`), and can otherwise contain letters (`a-z`, `A-Z`), numbers (`0-9`), underscores (`_`), apostrophes (`'`), or dashes (`-`).
> 
> > ##### Example
> > 
> > Creates an attribute set of two variables, `b` contains default value.
> > 
> > ```nix
> > {
> > 	a;
> > 	b = "Bar";
> > }
> > ```
> > 
> 
> > [!related]
> > 
> > [attribute-selection](programming/languages/nix/operators/attribute-selection)

## References

> https://nixos.org/manual/nix/stable/language/values
