# Startup Files

If any of the files exist but cannot be read, bash reports an error. Tildes are expanded in filenames as described in [Tilde Expansion](bash/shell/expansions/tilde-expansion).

#### Invoked as an interactive login shell, or with --login

When Bash is invoked as an interactive login shell, or as a non-interactive shell with the --login option, it first reads and executes commands from the file /etc/profile, if that file exists.

After reading that file, it looks for ~/.bash_profile, ~/.bash_login, and ~/.profile, in that order, and reads and executes commands from the first one that exists and is readable. The --noprofile option may be used when the shell is started to inhibit this behavior.

When an interactive login shell exits, or a non-interactive login shell executes the `exit` builtin command, Bash reads and executes commands from the file ~/.bash_logout, if it exists.

#### Invoked as an interactive non-login shell

Bash reads and executes commands from ~/.bashrc, if that file exists. This may be inhibited by using the --norc option. The --rcfile file option will force Bash to read and execute commands from file instead of ~/.bashrc.

#### Invoked non-interactively

Bash looks for the variable `BASH_ENV` in the environment, expands its value if it appears there, and uses the expanded value as the name of a file to read and execute, but the value of the `PATH` variable is not used to search for the filename.

#### Invoked with name `sh`

If Bash is invoked with the name `sh`, it tries to mimic the startup behavior of historical versions of `sh` as closely as possible, while conforming to the POSIX standard as well.

#### Invoked in POSIX mode

When Bash is started in POSIX mode, as with the `--posix` command line option, it follows the POSIX standard for startup files. In this mode, interactive shells expand the `ENV` variable and commands are read and executed from the file whose name is the expanded value. No other startup files are read.

## References

- https://www.gnu.org/software/bash/manual/bash.html#Bash-Startup-Files
