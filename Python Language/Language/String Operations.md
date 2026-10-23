### Creation & Basic Info
- Strings are immutable sequences of characters
- Defined with `'single'`, `"double"`, or `'''triple'''` quotes
---
### Common Operations
|Operation|Syntax|Example|Result|
|---|---|---|---|
|Concatenation|`+`|`"hi" + " there"`|`"hi there"`|
|Repetition|`*`|`"ab" * 3`|`"ababab"`|
|Length|`len()`|`len("hello")`|`5`|
|Indexing|`[i]`|`"hello"[1]`|`"e"`|
|Slicing|`[s:e:step]`|`"hello"[1:4]`|`"ell"`|
|Membership|`in`|`"he" in "hello"`|`True`|
---
### Built-in Methods
|Method|Purpose|Example|Result|
|---|---|---|---|
|`.upper()`|Uppercase|`"hi".upper()`|`"HI"`|
|`.lower()`|Lowercase|`"HI".lower()`|`"hi"`|
|`.strip()`|Remove whitespace|`" hi ".strip()`|`"hi"`|
|`.split(sep)`|Split to list|`"a,b".split(",")`|`["a","b"]`|
|`.join(iter)`|Join iterable|`",".join(["a","b"])`|`"a,b"`|
|`.replace(a,b)`|Replace substring|`"hi".replace("h","b")`|`"bi"`|
|`.find(sub)`|Find index|`"hello".find("l")`|`2`|
|`.count(sub)`|Count occurrences|`"hello".count("l")`|`2`|
|`.startswith()`|Check prefix|`"hi".startswith("h")`|`True`|
|`.endswith()`|Check suffix|`"hi".endswith("i")`|`True`|
|`.isdigit()`|All digits?|`"123".isdigit()`|`True`|
|`.isalpha()`|All letters?|`"abc".isalpha()`|`True`|
|`.title()`|Title case|`"hello world".title()`|`"Hello World"`|
|`.zfill(n)`|Zero-pad|`"5".zfill(3)`|`"005"`|
|`.center(n)`|Center align|`"hi".center(6)`|`" hi "`|
---
### String Formatting
|Method|Syntax|Notes|
|---|---|---|
|f-string|`f"Hello {name}"`|Fastest, modern (Python 3.6+)|
|`.format()`|`"Hello {}".format(name)`|Flexible, older style|
|`%` formatting|`"Hello %s" % name`|Legacy style|
---
### Useful Tips
- **Reverse a string:** `s[::-1]`
- **Check palindrome:** `s == s[::-1]`
- **Unicode support:** Python 3 strings are Unicode by default — handles **Hindi, Tamil, Telugu** and all Indian language scripts natively
- **Raw strings:** `r"\n"` treats backslash literally — useful for regex and file paths