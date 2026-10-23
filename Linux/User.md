In Linux, a **user** is a **security principal identified by a numeric UID** that the kernel uses to:  
- **Own processes**  
- **Own filesystem objects**  
- **Determine access rights**
A user is identified by a number, called UID.
A user contains the following information:
|Category|Information|Required|Stored in|
|---|---|---|---|
|Core identity|UID|Yes|`/etc/passwd`|
|Core identity|Real UID|Yes (runtime)|Kernel (process credentials)|
|Core identity|Effective UID|Yes (runtime)|Kernel|
|Core identity|Saved UID|Yes (runtime)|Kernel|
|Core identity|Filesystem UID|Yes (runtime)|Kernel|
|Group association|Primary GID|Yes|`/etc/passwd`|
|Group association|Supplementary groups|No|`/etc/group`, `/etc/gshadow`|
|Authentication|Password hash|No|`/etc/shadow`|
|Authentication|Password aging rules|No|`/etc/shadow`|
|Authentication|Account expiry|No|`/etc/shadow`|
|Authentication|Last password change|No|`/etc/shadow`|
|Account metadata|Username|Yes|`/etc/passwd`|
|Account metadata|Home directory|No|`/etc/passwd`|
|Account metadata|Login shell|No|`/etc/passwd`|
|Account metadata|GECOS (comment)|No|`/etc/passwd`|
|Authorization|File ownership|Yes|Filesystem metadata|
|Authorization|Permission bits|Yes|Filesystem metadata|
|Authorization|ACLs|No|Filesystem (extended attributes)|
|Authorization|Linux capabilities|No|Kernel / extended attributes|
|Session|Login name|No|Session state|
|Session|Active sessions|No|`/run/utmp`, `/var/log/wtmp`|
|Session|TTY / display|No|Session state|
|Session|Environment variables|No|Process memory|
|Resources|ulimits|No|Kernel / PAM|
|Resources|Disk quotas|No|Filesystem quota DB|
|Resources|cgroups|No|Kernel (cgroupfs)|
|Security|SELinux/AppArmor context|No|LSM metadata|
|Security|User namespace mappings|No|Kernel|
|Accounting|Login history|No|`/var/log/wtmp`|
|Accounting|Failed logins|No|`/var/log/btmp`|
|Accounting|Audit records|No|`/var/log/audit/`|
**User related commands:**
|Category|Command|Purpose|
|---|---|---|
|User management|`useradd`|Create a user|
|User management|`userdel`|Delete a user|
|User management|`usermod`|Modify user properties|
|User management|`newusers`|Bulk user creation|
|User management|`loginctl`|Manage user logins (systemd)|
|Password & account|`passwd`|Set or change password|
|Password & account|`chage`|Password aging and expiry|
|Password & account|`faillog`|Show failed login attempts|
|Password & account|`faillock`|Lock/unlock user accounts|
|Identity|`id`|Show UID, GID, groups|
|Identity|`whoami`|Show effective user|
|Identity|`logname`|Show login name|
|Identity|`getent passwd`|Lookup user via NSS|
|Group (user view)|`groups`|Show user’s groups|
|Group (user view)|`newgrp`|Switch primary group|
|Privilege|`su`|Switch user|
|Privilege|`sudo`|Run command as another user|
|Session|`login`|Start login session|
|Session|`logout` / `exit`|End session|
|Session|`who`|Show logged-in users|
|Session|`w`|Show active users and activity|
|Session|`last`|Login history|
|Session|`lastlog`|Last login per user|
|Resources|`ulimit`|User process limits|
|Resources|`quota`|Disk usage per user|
|Files & ownership|`chown`|Change file owner|
|Files & ownership|`chmod`|Change permissions|
|Files & ownership|`setfacl`|Set ACLs|
|Files & ownership|`getfacl`|View ACLs|
|System files|`vipw`|Safely edit `/etc/passwd`|
|System files|`vigr`|Safely edit `/etc/group`|
**User related commands clarifications:**
Difference between `useradd` and `adduser` commands:
|Aspect|`useradd`|`adduser`|
|---|---|---|
|Type|**Low-level system command**|**High-level wrapper script**|
|Standard|POSIX / shadow-utils|Distro-specific|
|Availability|Present on all Linux systems|May not exist on some distros|
|Interactivity|**Non-interactive** by default|**Interactive** (asks questions)|
|Defaults handling|Minimal, strict|Smart defaults|
|Creates home dir|**No** (unless `-m`)|**Yes** (by default)|
|Copies `/etc/skel`|Only if configured|**Yes**|
|Sets password|No|Yes (prompts)|
|Error-prone|Higher (easy to forget flags)|Lower|
|Script automation|**Best choice**|Not ideal|
|Under the hood|Directly edits user DB|Calls `useradd` internally|
Other commands like `userdel` and `deluser` have the same differences. Here is a list of such user related commands:
|Low-level (system)|High-level (helper)|What they do|
|---|---|---|
|`useradd`|`adduser`|Create users|
|`userdel`|`deluser`|Delete users|
|`usermod`|`adduser` (modify mode)|Modify users|
|`passwd`|`adduser` (password step)|Set passwords|
# References
- ChatGPT