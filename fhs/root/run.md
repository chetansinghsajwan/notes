---
status: completed
---

# FHS `/run` dir

This directory contains system information data describing the system since it was booted. Files under this directory must be cleared (removed or truncated as appropriate) at the beginning of the boot process.

The purposes of this directory were once served by `/var/run`. In general, programs may continue to use `/var/run` to fulfill the requirements set out for `/run` for the purposes of backwards compatibility. Programs which have migrated to use `/run` should cease their usage of `/var/run`, except as noted in the section on `/var/run`.

Programs may have a subdirectory of `/run`; this is encouraged for programs that use more than one run-time file. Users may also have a subdirectory of `/run`, although care must be taken to appropriately limit access rights to prevent unauthorized use of `/run` itself and other subdirectories. [[17]](https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html#ftn.idm236088274176)

### 3.15.2. Requirements

Process identifier (PID) files, which were originally placed in `/etc`, must be placed in `/run`. The naming convention for PID files is `<program-name>.pid`. For example, the **crond** PID file is named `/run/crond.pid`.

The internal format of PID files remains unchanged. The file must consist of the process identifier in ASCII-encoded decimal, followed by a newline character. For example, if **crond** was process number 25, `/run/crond.pid` would contain three characters: two, five, and newline.

Programs that read PID files should be somewhat flexible in what they accept; i.e., they should ignore extra whitespace, leading zeroes, absence of the trailing newline, or additional lines in the PID file. Programs that create PID files should use the simple specification located in the above paragraph.

System programs that maintain transient UNIX-domain sockets must place them in this directory or an appropriate subdirectory as outlined above.