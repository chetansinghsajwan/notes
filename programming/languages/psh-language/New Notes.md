PowerShell _s_cripting language is designed especially for system administrators.
### How Powershell is different
Most shells operate by executing a command or utility in a new process, and presenting the results to the user as text. These shells also have commands that are built into the shell and run in the shell process. Because there are few built-in commands, many utilities have been created to supplement them. PowerShell is very different. Instead of processing text, the shell processes objects. PowerShell also includes a large set of built-in commands with each having a consistent interface and these can work with user-written commands.
An _object_ is a data entity that has _properties_ and _methods._
A major advantage of using objects is that it is much easier to _pipeline_ commands; that is, to write the output of one command to another command as input. (In a traditional command-line environment, the text output from one command needs to be manipulated to meet the input format of another.)
  
There are four kinds of commands in PowerShell
- **Script**
    
    A file of commands is called a _script_. By convention, a script has a filename extension of .ps1. The top-most level of a PowerShell program is a script, which, in turn, can invoke other commands.
    
- **Procedures**
    
    PowerShell supports modular programming via named procedures. A procedure written in PowerShell is called a _function_, while an external procedure made available by the execution environment (and typically written in some other language) is called a _method_.
    
- **Cmdlet**
    
    A _cmdlet_ is a simple, single-task command-line tool. Although a cmdlet can be used on its own, the full power of cmdlets is realized when they are used in combination to perform complex tasks.
    
- **Native Command**
    
    A _native command_ is part of the host environment.
    
  
Each time the PowerShell runtime environment begins execution, it begins what is called a _session_. Commands then execute within the context of that session.