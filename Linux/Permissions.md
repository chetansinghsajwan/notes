In Linux, **permissions are metadata bits attached to filesystem objects** that the kernel uses to **allow or deny operations** based on a process’s **UID and GID**.
Permissions are stored in 12 bit flags.
```
[ SUID | SGID | STICKY | rwx | rwx | rwx ]
                         user group others
```
There are 4 groups of permissions:  
1. 3 special permissions  
2. 3 basic permissions for file’s owner  
3. 3 basic permissions for file’s group  
4. 3 basic permissions for others (everyone else)
**Basic permissions**
|Permission|Symbol|Meaning|
|---|---|---|
|Read|`r`|View file content / list directory|
|Write|`w`|Modify file / create-delete files in directory|
|Execute|`x`|Run file / enter directory|
**Special permissions**
|Permission|Symbol|Purpose|
|---|---|---|
|SUID|`s`|Run file as file owner|
|SGID|`s`|Run as group / inherit group|
|Sticky bit|`t`|Only owner can delete files|
**Commands related to permissions:**
|Category|Command|What it does|
|---|---|---|
|View|`ls -l`|Show permissions|
|Change basic perms|`chmod`|Set `rwx` / numeric perms|
|Change ownership|`chown`|Change file owner (UID)|
|Change group|`chgrp`|Change file group (GID)|
|Defaults|`umask`|Set default permissions|
|Special perms|`chmod`|Set SUID / SGID / sticky|
|Inspect|`stat`|Show inode + perms details|