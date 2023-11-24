[[New Notes]]
PowerShell _s_cripting language is designed especially for system administrators.
It's also used to build, test, and deploy solutions, often in CI/CD environments. PowerShell is built on the .NET Common Language Runtime (CLR). All inputs and outputs are .NET objects. No need to parse text output to extract information from output. The PowerShell scripting language includes the following features:
- Extensible through [functions](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions_advanced), [classes](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_classes), [scripts](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_scripts), and [modules](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_modules)
- Extensible [formatting system](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_format.ps1xml) for easy output
- Extensible [type system](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_types.ps1xml) for creating dynamic types
- Built-in support for common data formats like [CSV](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/convertfrom-csv), [JSON](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/convertfrom-json), and [XML](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/convertto-xml)
In PowerShell, administrative tasks are generally performed via _cmdlets_ (pronounced _command-lets_), which are specialized .NET [classes](https://www.wikiwand.com/en/Class_(computer_programming)) implementing a particular operation. These work by accessing data in different data stores, like the [file system](https://www.wikiwand.com/en/File_system) or [Windows Registry](https://www.wikiwand.com/en/Windows_Registry), which are made available to PowerShell via _providers_. Third-party developers can add cmdlets and providers to PowerShell. Cmdlets may be used by scripts, which may in turn be packaged into modules. Cmdlets work in tandem with the .NET [API](https://www.wikiwand.com/en/Application_programming_interface).
  
## Cmdlets
Compiled commands in PowerShell are called cmdlets.
- Cmdlets names have the form of singular "Verb-Noun" commands to make them easily discoverable.
- For example, the cmdlet for determining what processes are running is `Get-Process` and the cmdlet for retrieving a list of services and their statuses is `Get-Service`.
## **Get-Help**
`Get-Help` is a multipurpose command. `Get-Help` helps you learn how to use commands once you find them. `Get-Help` can also be used to help locate commands, but in a different and more indirect way when compared to `Get-Command`.
When `Get-Help` is used to locate commands, it first searches for wildcard matches of command names based on the provided input. If it doesn't find a match, it searches through the help topics themselves, and if no match is found an error is returned. Contrary to popular belief, `Get-Help` can be used to find commands that don't have help topics.
Help information is grouped as
- NAME
- SYNOPSIS
- SYNTAX
- DESCRIPTION
- RELATED LINKS
- REMARKS
  
- A parameter that doesn't require a value is called a switch parameter. When a switch parameter is specified, its value is true and when it's not, its value is false.
# References

> [!info] What is PowerShell? - PowerShell  
> This article is an introduction to the PowerShell scripting environment and its features.  
> [https://learn.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7.3](https://learn.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7.3)  

> [!info] Wikiwand - PowerShell  
> PowerShell is a task automation and configuration management program from Microsoft, consisting of a command-line shell and the associated scripting language.  
> [https://www.wikiwand.com/en/PowerShell](https://www.wikiwand.com/en/PowerShell)  

> [!info] Learn X in Y Minutes: Scenic Programming Language Tours  
> Get the code:  
> [https://learnxinyminutes.com/docs/powershell/](https://learnxinyminutes.com/docs/powershell/)