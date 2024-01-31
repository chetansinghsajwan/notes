# Redirections

Redirection simply means capturing output from a file, command, program, script, or even code block within a script and sending it as input to another file, command, program, or script.

Redirections are processed in the order they appear, from left to right.

##### Redirecting Input

**Syntax:** `[n] < word`

Causes the file whose name results from the expansion of `word` to be opened for reading on file descriptor `n`.

By default `n` is `0` (stdin).

##### Redirecting Output

**Syntax:** `[n] > [|] word`

Causes the file whose name results from the expansion of `word` to be opened for writing on file descriptor `n`

If the file does not exist it is created and if it does exist it is truncated to zero size.

By default `n` is `1` (stdout).

Redirecting standard output to a file that already exists would overwrite the existing file content which will result in data loss. This process of over-writing existing data is called clobbering. In order to prevent overwriting, the shell provides an option called “noclobber”

##### Appending Redirected Output

**Syntax:** `[n] >> word`

Causes the file whose name results from the expansion of `word` to be opened for appending on file descriptor `n`.

By default `n` is `1` (stdout).

If the file does not exist it is created.

##### Redirecting Standard Output and Standard Error

**Syntax:** `&> word` or `>& word`

The first form is preferred.

This construct allows both the stdout and the stderr to be redirected to the file whose name is the expansion of word.

##### Appending Standard Output and Standard Error

**Syntax:** `&>> word`

This construct allows both the stdout and the stderr to be appended to the file whose name is the expansion of word.

##### Here Documents

**Syntax:**

[n]<<[-]word
        here-document
delimiter

This type of redirection instructs the shell to read input from the current source until a line containing only word (with no trailing blanks) is seen. All of the lines read up to that point are then used as the standard input (or file descriptor n if n is specified) for a command.

##### Here Strings
##### Duplicating File Descriptors
##### Moving File Descriptors
##### Opening File Descriptors for Reading and Writing

**Syntax:** `[n]<> word`

Causes the file whose name is the expansion of word to be opened for both reading and writing on file descriptor n, or on file descriptor 0 if n is not specified. If the file does not exist, it is created.

## References

- https://www.gnu.org/software/bash/manual/bash.html#Redirections
- https://tldp.org/LDP/abs/html/io-redirection.html