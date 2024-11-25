# Startup

Bash loads different files based on how `bash` was run.

---
**If the shell is run in [login](bash/shell/modes/login) mode**

It reads and executes commands from the file `/etc/profile`, if that file exists. After reading that file, it looks for `~/.bash_profile`, `~/.bash_login`, and `~/.profile`, in that order, and reads and executes commands from the first one that exists and is readable.

The [`--noprofile`](bash/shell/options/noprofile) option may be used when the shell is started to inhibit this behavior.

**If the shell is run in [non-interactive](bash/shell/modes/interactive) mode**

It looks for the variable `BASH_ENV` in the environment, expands its value if it appears there, and uses the expanded value as the name of a file to read and execute. But the value of the `PATH` variable is not used to search for the filename.

---
**If the shell is run in [interactive](bash/shell/modes/interactive) and [non-login](bash/shell/modes/login) mode**

It reads and executes commands from `~/.bashrc`, if that file exists.

This may be inhibited by using the [`--norc`](bash/shell/options/norc) option.

The [`--rcfile`](bash/shell/rcfile) file option will force bash to read and execute commands from that file instead of `~/.bashrc`.

---
**If the shell is run in [non-interactive](bash/shell/modes/interactive) and [non-login](bash/shell/modes/login) mode**

it looks for the variable `BASH_ENV` in the environment, expands its value if it appears there, and uses the expanded value as the name of a file to read and execute. But the value of the `PATH` variable is not used to search for the filename.

---
**If the shell is run as [`sh`](bash/shell/mode/sh) command**

It tries to mimic the startup behavior of historical versions of `sh` as closely as possible, while conforming to the POSIX standard as well.

If the shell is a login shell, it first attempts to read and execute commands from `/etc/profile` and `~/.profile`, in that order. The `--noprofile` option may be used to inhibit this behavior. When invoked as an interactive shell, it looks for the variable `ENV`, expands its value if it is defined, and uses the expanded value as the name of a file to read and execute.

Bash enters posix mode after the startup files are read.

**If the shell is run in [posix](bash/shell/modes/posix.md) mode**

It follows the posix standard for startup files. In this mode, interactive shells expand the `ENV` variable and commands are read and executed from the file whose name is the expanded value.

## References

- https://www.gnu.org/software/bash/manual/bash.html#Bash-Startup-Files
