# Golang

---
**GOROOT**: The env variable specifies where go sdk is installed. You don't need to set this variable as go will automatically figure it out from where it is invoked.

`$GOROOT/bin` needs to be added to `PATH` though.

---
**GOPATH**: This env variable specifies where go will store its artificats like downloading and installing packages.

`$GOPATH/bin` needs to be added to `PATH` to find binaries installed by go.

---
**Docker Installation**: Follow the following steps:

1. Create a new dir and set this path as `GOROOT` env.
2. Create a new dir and set this path as `GOPATH` env.
3. Add `$GOROOT/bin` and `$GOPATH/bin` to `PATH` env.
4. Download and extract golang archive in `GOROOT`.

## References

- Chatgpt
