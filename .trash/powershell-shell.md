# Powershell Shell

PowerShell is a modern command shell that includes the best features of other popular shells. Unlike most shells that only accept and return text, PowerShell accepts and returns .NET objects. The shell includes the following features:
- Robust command-line [history](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_history)
- Tab completion and command prediction (See [about_PSReadLine](https://learn.microsoft.com/en-us/powershell/module/psreadline/about/about_psreadline))
- Supports command and parameter [aliases](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_aliases)
- [Pipeline](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_pipelines) for chaining commands
- In-console [help](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/get-help) system, similar to Unix `man` pages

## What version of PowerShell am I running?

There are a number of automatic variables in PowerShell that store state information. One of these variables is `$PSVersionTable`, which contains a hashtable that can be used to display the relevant PowerShell version information:

```shell-instance
$PSVersionTable
Name                           Value
----                           -----
PSVersion                      5.1.19041.1
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.19041.1
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```
## **Execution Policy**
Contrary to popular belief, the execution policy in PowerShell is not a security boundary. It's designed to prevent a user from unknowingly running a script. A determined user can easily bypass the execution policy in PowerShell.

- Table 1-2 shows the default execution policy for current Windows operating systems.
    
    |   |   |
    |---|---|
    |Windows Operating System Version|Default Execution Policy|
    |Server 2019|Remote Signed|
    |Server 2016|Remote Signed|
    |Windows 10|Restricted|
    
- Regardless of the execution policy setting, any PowerShell command can be run interactively. The execution policy only affects commands running in a script.
- The `Get-ExecutionPolicy` cmdlet is used to determine what the current execution policy setting is and the `Set-ExecutionPolicy` cmdlet is used to change the execution policy.
- It is recommended to use the **RemoteSigned** policy, which requires downloaded scripts to be signed by a trusted publisher in order to be run.
- PowerShell scripts can't be run at all when the execution policy is set to **Restricted.** This is the default setting on all Windows client operating systems