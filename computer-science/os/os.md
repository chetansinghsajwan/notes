Really good explanation on **Quora** by ABC on difference between **OS** and **Kernel**.

> Originally computers did not have Operating Systems. Every time you wanted to run a program you would load that program into the computer and point the Programmer Counter to the start of the program. If you needed access to a printer or a tape drive you had to write the code to fill the buffers then transmit the buffer to the device. Timing was always a big issue.
> 
> Originally computers did not have Operating Systems. Every time you wanted to run a program you would load that program into the computer and point the Programmer Counter to the start of the program. If you needed access to a printer or a tape drive you had to write the code to fill the buffers then transmit the buffer to the device. Timing was always a big issue.
> 
> As time went on people found that others had written modules that would address the problems of communicating with the desired device much more completely than you could writing the device access yourself, and they started adding these modules to their programs each time they loaded a program that needed device access. Eventually someone came up with the idea to load these modules permanently in memory so they would be available to all programs when they were loaded. This collection of permanently available modules was called the Operating System.
> 
> As time went on people found that others had written modules that would address the problems of communicating with the desired device much more completely than you could writing the device access yourself, and they started adding these modules to their programs each time they loaded a program that needed device access. Eventually someone came up with the idea to load these modules permanently in memory so they would be available to all programs when they were loaded. This collection of permanently available modules was called the Operating System.
> 
> This led to the idea of memory management, because you didn’t want your program writing over the areas of memory where the Operating System was stored. That would have made it impossible for you to access the modules you needed to communicate with the devices your Application Program needed.
> 
> Eventually the Operating System took on tasks such as communication with printers, storage devices, terminals, managing when programs would run, managing multi-tasking environments, etc.
> 
> Until the early 1970s Operating Systems were written in Assembly Language and each OS was specific to the machine it was written for. The Operating System for a PDP-11 would not run on an IBM-370, or a Honeywell-6000. In fact the Operating System for a PDP-8 would not run on a PDP-11, etc.
> 
> In the early 1970s scientists (Kernighan, Ritchie, McIlroy, and Neumann) at Bell Labs (Murray Hill, New Jersey) were doing research on an Operating System they called Unics (Later UNIX) and they decided to re-code parts of the system in C, a language that at that time was under development by Dennis Ritchie with assistance from Ken Thompson.
> 
> Writing UNIX in C allowed parts of the Operating System to be ported to other machines without having to re-design and re-write the entire operating system. The parts of the Operating System that could not be ported because they involved direct access to hardware that would be different for each type of machine was called the Kernel. The Kernel is a part of the Operating System, but is specific to the hardware on which it’s running. The rest of the Operating System can be written once in a high level language (like C), then ported to a new machine by re-compiling it in the new machine. The Operating System still takes care of Memory Management, Device Management and Communication, Execution Management, etc.
> 
> As time went on people started including other programs such as compilers, utilities, editors, etc. in their definition of the phrase Operating System. Often these are programs that are installed as a standard part of the Operating System. You will even find some people including Web Browsers, Spreadsheets, and Word Processors in their definition of Operating System. To me compilers, Browsers, Word Processors, etc. are Application Programs and are not really part of the Operating System.
> 
> The Operating System is the set of programs that reside in the computer that manage the computer’s resources, allow programs to access those resources, and arbitrate between programs trying to use those resources simultaneously. The Kernel is the part of the Operating System that was written specifically for the machine on which it’s running and can not easily be ported to another machine.

## List of major Operating systems

- [[unix-os]]
- [[linux]]
- [[windows-os]]
- [[GNU Operating System]]