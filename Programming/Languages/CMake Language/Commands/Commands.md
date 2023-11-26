#### Command Invocations

A _command invocation_ is a name followed by paren-enclosed arguments separated by whitespace.

Command names are case-insensitive.

###### Syntax

```
command_invocation  ::=  space* identifier space* '(' arguments ')'
identifier          ::=  <match '[A-Za-z_][A-Za-z0-9_]*'>
arguments           ::=  argument? separated_arguments*
separated_arguments ::=  separation+ argument? |
                         separation* '(' arguments ')'
separation          ::=  space | line_ending
```
###### Example

```cmake
add_executable(hello world.c)
```

#### Command Arguments

There are three types of arguments within **Command Invocations**.

1. Bracket Argument
2. Quoted Argument
3. Unquoted Argument

##### Bracket Argument

A _bracket argument_, inspired by [Lua](http://www.lua.org/) long bracket syntax, encloses content between opening and closing "brackets" of the same length:   

###### Syntax
```
	bracket_argument ::=  bracket_open bracket_content bracket_close
    bracket_open     ::=  '[' '='* '['
	bracket_content  ::=  <any text not containing a bracket_close the same
		number of '=' as the bracket_open>
    bracket_close    ::=  ']' '='* ']'
```
    
An opening bracket is written `[` followed by zero or more  followed by `[`. The corresponding closing bracket is written `]` followed by the same number of `=` followed by `]`.

Brackets do not nest. A unique length may always be chosen for the opening and closing brackets to contain closing brackets of other lengths.

Bracket argument content consists of all text between the opening and closing brackets, except that one newline immediately following the opening bracket, if any, is ignored.
    
No evaluation of the enclosed content, such as [Escape Sequences](https://cmake.org/cmake/help/latest/manual/cmake-language.7.html#escape-sequences) or [Variable References](https://cmake.org/cmake/help/latest/manual/cmake-language.7.html#variable-references), is performed.
    
A bracket argument is always given to the command invocation as exactly one argument.
    
###### Example

```
    message([=[
    	This is the first line in a bracket argument with bracket length 1.
    	No \-escape sequences or ${variable} references are evaluated.
    	This is always one argument even though it contains a ; character.
    	The text does not end on a closing bracket of length 0 like ]].
    	It does end in a closing bracket of length 1.
    	]=])
```

##### Quoted Argument

A _quoted argument_ encloses content between opening and closing double-quote characters.

###### Syntax
    
```
    quoted_argument     ::=  '"' quoted_element* '"'
    quoted_element      ::=  <any character except '\' or '"'> |
                             escape_sequence |
                             quoted_continuation
    quoted_continuation ::=  '\' newline
```
 
Quoted argument content consists of all text between opening and closing quotes. Both [Escape Sequences](https://cmake.org/cmake/help/latest/manual/cmake-language.7.html#escape-sequences) and [Variable References](https://cmake.org/cmake/help/latest/manual/cmake-language.7.html#variable-references) are evaluated.

A quoted argument is always given to the command invocation as exactly one argument.

###### Example

```
    message("This is a quoted argument containing multiple lines.
    This is always one argument even though it contains a ; character.
    Both \\-escape sequences and ${variable} references are evaluated.
    The text does not end on an escaped double-quote like \".
    It does end in an unescaped double quote.
    ")
```

###### Example

```
	message("\
    This is the first line of a quoted argument. \
    In fact it is the only line but since it is long \
    the source code uses line continuation.\
    ")
```

 The final `\` on any line ending in an odd number of backslashes is treated as a line continuation and ignored along with the immediately following newline character.

##### Unquoted Argument

An _unquoted argument_ is not enclosed by any quoting syntax.

It may not contain any whitespace, `(`, `)`, `#`, `"`, or `\` except when escaped by a backslash.

###### Syntax

```
    unquoted_argument ::=  unquoted_element+ | unquoted_legacy
    unquoted_element  ::=  <any character except whitespace or one of '()#"\'> |
                           escape_sequence
    unquoted_legacy   ::=  <see note in text>
```
  
###### Commands List
[[block command]]
[[break command]]
[[cmake_minimum_required command]]
[[project command]]
[[function command]]
[[macro command]]
[[cmake_policy command]]
[[foreach command]]
[[while command]]
[[continue command]]
[[else command]]
[[if command]]
[[elseif command]]
[[endif command]]
[[include command]]
[[command 2]]
[[command 3]]
[[command 4]]
[[command 5]]
[[command 6]]
[[command 7]]
[[command 8]]
[[command 9]]
# References
	
> [!info] cmake-language
> [https://cmake.org/cmake/help/latest/manual/cmake-language.7.html](https://cmake.org/cmake/help/latest/manual/cmake-language.7.html)