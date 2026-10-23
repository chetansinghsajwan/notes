Bash Shell Expansions
Bash Shell Language Constructs
Bash Shell Modes
[[Command Search and Execution|Bash Shell Command Search and Execution]]
[[Commands|Bash Shell Commands]]
[[Interactive Shell|Bash Shell Interactive Shell]]
[[Looping Constructs|Bash Shell Looping Constructs]]
[[Pattern Matching|Bash Shell Pattern Matching]]
[[Redirections|Bash Shell Redirections]]
[[Reserved Words|Bash Shell Reserved Words]]
[[Shutdown|Bash Shell Shutdown]]
[[Startup Files|Bash Shell Startup Files]]
[[Startup|Bash Shell Startup]]
[[Bash Shell/Learning Sources|Bash Shell Learning Sources]]
---
## How it works?
When the shell reads input, it proceeds through a sequence of operations. If the input indicates the beginning of a comment, the shell ignores the comment symbol (‘\#’), and the rest of that line.
Otherwise, roughly speaking, the shell reads its input and divides the input into words and operators, employing the quoting rules to select which meanings to assign various words and characters.
The shell then parses these tokens into commands and other constructs, removes the special meaning of certain words or characters, expands others, redirects input and output as needed, executes the specified command, waits for the command’s exit status, and makes that exit status available for further inspection or processing.
# References

> [!info] Bash Reference Manual  
>  
> https://www.gnu.org/software/bash/manual/bash.html