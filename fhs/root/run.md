# FHS `/run` dir

This directory contains system information data describing the system since it was booted.

The purposes of this directory were once served by `/var/run`. In general, programs may continue to use `/var/run` to fulfill the requirements set out for `/run` for the purposes of backwards compatibility.

Programs may have a subdirectory of `/run`. This is encouraged for programs that use more than one run-time file. Users may also have a subdirectory of `/run`.

`/run` should not be writable for unprivileged users. It is a major security problem if any user can write in this directory. User-specific subdirectories should be writable only by each directory's owner.

Process identifier (PID) files, which were originally placed in `/etc`, must be placed in `/run`. The naming convention for PID files is `<program-name>.pid`. For example, the **crond** PID file is named `/run/crond.pid`.

The internal format of PID files remains unchanged. The file must consist of the process identifier in ASCII-encoded decimal, followed by a newline character. For example, if **crond** was process number 25, `/run/crond.pid` would contain three characters: two, five, and newline.

Programs that read PID files should be somewhat flexible in what they accept; i.e., they should ignore extra whitespace, leading zeroes, absence of the trailing newline, or additional lines in the PID file. Programs that create PID files should use the simple specification located in the above paragraph.

## References

- https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html#runRuntimeVariableData
