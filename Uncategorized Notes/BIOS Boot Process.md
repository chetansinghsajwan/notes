## BIOS
When your PC is first powered up, a lot happens. Electrical components of the PC are initially responsible for bringing your computer to life, as debouncing circuits take your push of the power button and trigger a switch that activates the power supply and directs current from the PSU to the motherboard and, mainly through it, to all the various components of your PC. As each individual component receives life-giving electricity, it is powered up and brought online to its initial state. The startup routines and overall functionality of the simpler components like the RAM and PSU is hardwired into them as a series of logic circuits (AND/NAND and OR/NOR gates), while more complicated parts such as the video card have their own micro-controllers that act as mini-CPUs, controlling the hardware and interfacing with the rest of your PC to delegate and oversee the work.
### **The POST Process**
Once your PC has been powered on, the BIOS begins its work as part of the POST (Power-On Self Test) process. It bridges all the various parts of your PC together, and interfaces between them as required, setting up your video display to accept basic VGA and show it on the screen, initializing the memory banks and giving your CPU access to all the hardware. It scans the IO buses for attached hardware, and identifies and maps access to the hard disks you have connected to your PC. The BIOS on newer motherboards is smart enough to even recognize and identify USB devices, such as external drives and USB mice, letting you boot from USB sticks and use your mouse in legacy software.
During the POST procedure, quick tests are conducted where possible, and errors caused by incompatible hardware, disconnected devices, or failing components are often caught. It’s the BIOS that’s responsible for a variety of error messages such as “keyboard error or no keyboard present” or warnings about mismatched/unrecognized memory. At this point, the majority of the BIOS’ work has completed and it’s almost ready to move on to the next stage of the boot process. The only thing left is to run what are called “Add-On ROMs”: some hardware attached to the motherboard might require user intervention to complete its initialization and the BIOS actually hands off control of the entire PC to software routines coded into hardware like the video card or RAID controllers. They assume control of the computer and its display, and let you do things like set up RAID arrays or configure display settings before the PC has even truly finished powering up. When they’re done executing, they pass control of the computer back to the BIOS and and the PC enters a basic, usable state and is ready to begin.
### **BIOS Boot Handoff**
After having configured the basic input and output devices of your PC, the BIOS now enters the final stages where it’s still in control of your computer. At this point, you’ll normally be presented with an option to quickly hit a key to enter the BIOS setup from where you can configure hardware settings and control how your PC boots. If you choose nothing, the BIOS will begin the first step in actually “booting” your PC using the default settings.
Earlier we mentioned that an important part of the BIOS’ work is to detect and map connected hard disks. This list now comes in handy, as the BIOS will load a very small program from the first hard disk to the memory and tell the CPU to execute its contents, handing off control of the computer to whatever is on the hard drive and ending its active role in loading your PC. This hard drive is known as “the boot device,” “startup disk,” or “drive 0” and can usually be picked or set in the BIOS setup.
# To Read

> [!info] How BIOS Works  
> One of the most common uses of Flash memory is for the basic input/output system of your computer, commonly known as the BIOS.  
> [https://computer.howstuffworks.com/bios.htm](https://computer.howstuffworks.com/bios.htm)  

> [!info] Boot Sequence - OSDev Wiki  
> When a computer is switched on or reset, it runs through a series of diagnostics called POST - Power-On Self-Test.  
> [https://wiki.osdev.org/Boot_Sequence](https://wiki.osdev.org/Boot_Sequence)  

> [!info] Booting  
> In computing, booting is the process of starting a computer as initiated via hardware such as a button or by a software command.  
> [https://en.wikipedia.org/wiki/Booting](https://en.wikipedia.org/wiki/Booting)  
# References

> [!info] The BIOS/MBR Boot Process  
> Regardless of the computer or operating system, standard (“IBM-compatible”) desktop PCs and laptops all power on and start up using one of two ways: the traditional BIOS-MBR method and …  
> [https://neosmart.net/wiki/mbr-boot-process/](https://neosmart.net/wiki/mbr-boot-process/)