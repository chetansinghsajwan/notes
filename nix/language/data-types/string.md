# `string` type

Strings can be written in three ways.

1. The most common way is to enclose the string between double quotes, e.g., `"foo bar"`.
   
   Strings can span multiple lines.
   
   The special characters `"` and `\` and the character sequence `${` must be escaped by prefixing them with a backslash (`\`).
   
   Newlines, carriage returns and tabs can be written as `\n`, `\r` and `\t`, respectively.
   
   You can include the results of other expressions into a string by enclosing them in `${ }`, a feature known as [string interpolation](programming/languages/nix/string-interpolation).

2. The second way to write string literals is as an _indented string_, which is enclosed between pairs of _double single-quotes_, like so:
   
   ```nix
   ''
	   This is the first line.
		   This is the second line.
		   This is the third line.
   ''
   ```

3. Finally, as a convenience, [URIs](uri.md) can be written as is, without quotes.

    ---
    
    **Example**
    
    The string `"http://example.org/foo.tar.bz2"` can also be written as just `http://example.org/foo.tar.bz2` without any quotes.
    
    ---

## References

- https://nixos.org/manual/nix/stable/language/values#type-string