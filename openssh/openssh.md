# OpenSSH

OpenSSH is an implementation of the [SSH Protocol](ssh/ssh).

It is developed by a few developers of the [OpenBSD Project](https://www.openbsd.org) and made available under a BSD-style license.

The goal of OpenSSH is: Since telnet and rlogin are insecure, all operating systems should ship with support for the SSH protocol included.

The OpenSSH suite consists of the following tools:

- Remote operations are done using [ssh](https://man.openbsd.org/ssh.1), [scp](https://man.openbsd.org/scp.1), and [sftp](https://man.openbsd.org/sftp.1).
- Key management with [ssh-add](https://man.openbsd.org/ssh-add.1), [ssh-keysign](https://man.openbsd.org/ssh-keysign.8), [ssh-keyscan](https://man.openbsd.org/ssh-keyscan.1), and [ssh-keygen](https://man.openbsd.org/ssh-keygen.1).
- The service side consists of [sshd](https://man.openbsd.org/sshd.8), [sftp-server](https://man.openbsd.org/sftp-server.8), and [ssh-agent](https://man.openbsd.org/ssh-agent.1).

OpenSSH is written for OpenBSD operating system. But there is a port of OpenBSD's [OpenSSH](https://openssh.com) to most Unix-like operating systems, including Linux, OS X and Cygwin at [github](https://github.com/openssh/openssh-portable).

To learn about what ssh specifications openssh implements, see https://www.openssh.com/specs.html.

## References

- https://www.openssh.com
