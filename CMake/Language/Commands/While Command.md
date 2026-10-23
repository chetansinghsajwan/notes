\#todo add info about break and continue commands.
Evaluate a group of commands while a condition is true.
[[Endwhile Command]] command is used to mark the end of the while loop.
The commands [[Break Command|cmake/language/commands/break]] and [[Continue Command]] provide means to escape from the normal control flow.
## Syntax
```
while(<condition>)
  <commands>
endwhile()
```
## Parameters

> `**<condition>**`
> 
> An expression of type [[Condition Expressions]].
## References
- https://cmake.org/cmake/help/latest/command/while.html