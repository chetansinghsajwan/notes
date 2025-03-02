# Git Store Credential Helper

Stores credentials in plain text format in files specified below.

If not set explicitly with `--file`, there are two files where git-credential-store will search for credentials in order of precedence:

- `~/.git-credentials`
	 
	User-specific credentials file.

- `$XDG_CONFIG_HOME/git/credentials`
	
	Second user-specific credentials file. If `$XDG_CONFIG_HOME` is not set or empty, `$HOME/.config/git/credentials` will be used. Any credentials stored in this file will not be used if `~/.git-credentials` has a matching credential as well. It is a good idea not to create this file if you sometimes use older versions of Git that do not support it.

For credential lookups, the files are read in the order given above, with the first matching credential found taking precedence over credentials found in files further down the list.

Credential storage will by default write to the first existing file in the list. If none of these files exist, `~/.git-credentials` will be created and written to.

When erasing credentials, matching credentials will be erased from all files.

The `.git-credentials` file is stored in plaintext. Each credential is stored on its own line as a URL like:

https://user:pass@example.com

No other kinds of lines (e.g. empty lines or comment lines) are allowed in the file, even though some may be silently ignored.
## References

- https://git-scm.com/docs/git-credential-store
