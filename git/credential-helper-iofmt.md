# Git Credential Helper Input/Output Format

`git credential` reads and/or writes (depending on the action used) credential information in its standard input/output. This information can correspond either to keys for which `git credential` will obtain the login information (e.g. host, protocol, path), or to the actual credential data to be obtained (username/password).

The credential is split into a set of named attributes, with one attribute per line. Each attribute is specified by a key-value pair, separated by an `=` (equals) sign, followed by a newline.

The key may contain any bytes except `=`, newline, or NUL. The value may contain any bytes except newline or NUL. A line, including the trailing newline, may not exceed 65535 bytes in order to allow implementations to parse efficiently.

Attributes with keys that end with C-style array brackets `[]` can have multiple values. Each instance of a multi-valued attribute forms an ordered list of values - the order of the repeated attributes defines the order of the values. An empty multi-valued attribute (`key[]=\n`) acts to clear any previous entries and reset the list.

In all cases, all bytes are treated as-is (i.e., there is no quoting, and one cannot transmit a value with newline or NUL in it). The list of attributes is terminated by a blank line or end-of-file.

See this [link](https://git-scm.com/docs/git-credential#IOFMT) for a list of attributes recognized by git. Unrecognized attributes and capabilities are silently discarded.

## References

- https://git-scm.com/docs/git-credential#IOFMT
