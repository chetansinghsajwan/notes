# Group

A **group** is a **numeric security identifier (GID)** that represents a **set of users**.    

The kernel uses **GIDs** to:
- check access permissions
- assign group ownership to files
- grant shared access to resources

| Category      | Information                      | Required       | Stored in            |
| ------------- | -------------------------------- | -------------- | -------------------- |
| Core identity | GID (Group ID)                   | Yes            | `/etc/group`         |
| Metadata      | Group name                       | Yes            | `/etc/group`         |
| Membership    | Member users (supplementary)     | No             | `/etc/group`         |
| Membership    | Group administrators             | No             | `/etc/gshadow`       |
| Security      | Group password (legacy)          | No             | `/etc/gshadow`       |
| Security      | Group access control             | No             | `/etc/gshadow`       |
| Usage         | Primary group assignment         | Yes (via user) | `/etc/passwd`        |
| Usage         | Supplementary group mapping      | No             | `/etc/group`         |
| Filesystem    | Group ownership of files         | Yes            | Filesystem metadata  |
| Filesystem    | SGID bit on directories          | No             | Filesystem metadata  |
| Runtime       | Effective GID (per process)      | Yes (runtime)  | Kernel               |
| Runtime       | Supplementary GIDs (per process) | No             | Kernel               |
| Resolution    | NSS source (files/LDAP/AD)       | No             | `/etc/nsswitch.conf` |

**Group related commands:**

| Category        | Command        | Purpose                                  |
| --------------- | -------------- | ---------------------------------------- |
| Group lifecycle | `groupadd`     | Create a group                           |
| Group lifecycle | `groupdel`     | Delete a group                           |
| Group lifecycle | `groupmod`     | Modify group name or GID                 |
| Membership      | `gpasswd`      | Manage group members & admins            |
| Membership      | `newgrp`       | Switch current primary group             |
| Information     | `getent group` | Lookup group via NSS                     |
| Information     | `groups`       | Show groups of current/user              |
| Information     | `id`           | Show group IDs (primary + supplementary) |
| Database edit   | `vigr`         | Safely edit `/etc/group`                 |
| Database edit   | `vipw -g`      | Safely edit `/etc/group`                 |

**Group related commands clarifications:**

Difference between `groupadd` and `addgroup` commands:

| Aspect                     | `groupadd`                   | `addgroup`                   |
| -------------------------- | ---------------------------- | ---------------------------- |
| Type                       | **Low-level system command** | **High-level helper script** |
| Standard                   | POSIX / shadow-utils         | Debian-family specific       |
| Availability               | Present on all Linux distros | Mainly Debian / Ubuntu       |
| Interactivity              | **Non-interactive**          | Can be interactive           |
| Defaults handling          | Minimal                      | Smart defaults               |
| GID selection              | Manual or auto               | Auto by default              |
| Error safety               | Lower (strict)               | Higher (guardrails)          |
| Script automation          | **Best choice**              | Not recommended              |
| Under the hood             | Directly edits group DB      | Calls `groupadd`             |
| Consistency across distros | **High**                     | **Low**                      |

Other commands like `groupdel` and `delgroup` have the same differences. Here is a list of such user related commands:

| Low-level (system) | High-level (helper)                | What they do                 |
| ------------------ | ---------------------------------- | ---------------------------- |
| `groupadd`         | `addgroup`                         | Create a group               |
| `groupdel`         | `delgroup`                         | Delete a group               |
| `groupmod`         | `addgroup` (modify mode)           | Modify group name / GID      |
| `gpasswd`          | `adduser` / `deluser` (group mode) | Add/remove users from groups |

# References

- ChatGPT
