# UEFI

(U)EFI or (Unified) Extensible Firmware Interface is a specification for x86, x86-64, ARM, and Itanium platforms that defines a software interface between the operating system and the platform firmware/BIOS. The original EFI was developed in the mid-1990s by Intel for use developing firmware/BIOS for Itanium platforms. In 2005 Intel transitioned the specification to a new working group called the Unified EFI Forum, consisting of companies such as AMD, Microsoft, Apple, and Intel itself. All modern PCs ship with UEFI firmware and UEFI is widely supported by both commercial and open source operating systems. Backwards compatibility is provided for legacy operating systems.

> [!important]  
> UEFI is not an implementation but an interface between operating system and platoform firmware.  
### Platform initialization
On a legacy system, BIOS performs all the usual platform  
initialization (memory controller configuration, PCI bus configuration  
and BAR mapping, graphics card initialization, etc.), but then drops  
into a backwards-compatible real mode environment. The bootloader must  
enable the A20 gate, configure a GDT and an IDT, switch to protected  
mode, and for x86-64 CPUs, configure paging and switch to long mode.
UEFI firmware performs those same steps, but it also enables the  
A20 gate and prepares either protected mode environment with flat  
segmentation (for 32-bit x86 processors) or a long mode environment with  
identity-mapped paging (for x86-64 processors).
Additionally, the platform initialization procedure of UEFI  
firmware is standardized. This allows UEFI firmware to be extended in a  
vendor-neutral way.