# Partitions

- `**boot**`
    
	This partition contains a kernel image and is created using `mkbootimg`. You can use a virtual partition to flash either image directly without flashing a new boot partition. This partition also contains the generic ramdisk in devices launched before Android 13.
    
    - _kernel_
        
        The virtual `kernel` partition overwrites the kernel (`zImage`, `zImage-dtb`, `Image.gz-dtb`) by writing the new kernel image over the old kernel image. If the development kernel supplied is incompatible, you might need to update the `vendor`, `system`, or `dtb` partition (if present) with associated kernel modules.
        
    - _ramdisk._ The virtual `ramdisk` partition overwrites the ramdisk by writing the new ramdisk image over the old ramdisk image.
    
    The overwrite operation determines the start location of the existing image in eMMC and copies the new image to that location. The new image (kernel or ramdisk) might be larger than the existing one; to make space, the bootloader can move data following the image or abandon the operation with an error.
    
- `**init_boot**`
    
    This partition contains the generic ramdisk for devices launching with Android 13 and beyond.
    
- `**system**`
    
    This partition contains the Android framework.
    
- `**odm**`
    
    This partition contains original design manufacturer (ODM) customizations to system-on-chip (SoC) vendor board-support packages (BSPs). Such customizations enable ODMs to replace or customize SoC components, and implement kernel modules for board-specific components, daemons, and ODM-specific features on hardware abstraction layers (HALs). This partition is optional; typically, it's used to contain customizations so that devices can use a single vendor image for multiple hardware SKUs. For details, see [ODM Partitions](https://source.android.com/docs/core/architecture/bootloader/partitions/odm-partitions).
    
- `**odm_dlkm**`
    
    This partition is dedicated to storing ODM kernel modules. Storing ODM kernel modules in the `odm_dlkm` partition (as opposed to the `odm` partition) makes it possible to update ODM kernel modules without updating the `odm` partition.
    
- `**recovery**`
    
    This partition stores the recovery image, which is booted during the OTA process. Devices that support [seamless updates](https://source.android.com/docs/core/ota/ab) can store the recovery images as a ramdisk contained in the `boot` or `init_boot` image (rather than a separate image).
    
- `**cache**`
    
    This partition stores temporary data and is optional if a device uses seamless updates. The cache partition doesn't need to be writable from the bootloader, but does need to be erasable. The partition size depends on the device type and the availability of space on `userdata`; typically, 50 MB–100 MB is sufficient.
    
- `**misc**`
    
    This partition is used by the recovery partition and is 4 KB or larger.
    
- `**userdata**`
    
    This partition contains user-installed apps and data, including customization data.
    
- `**metadata**`
    
    This partition is used to store the metadata encryption key when the device uses [metadata encryption](https://source.android.com/docs/security/features/encryption/metadata). The size is 16 MB or larger. It is not encrypted and its data is not snapshotted. It is erased when the device is factory reset. Usage of this partition is strictly limited.
    
- `**vendor**`
    
    This partition contains any binary that isn't distributable to AOSP. If the device doesn't contain proprietary information, you can omit this partition.
    
- `**vendor_dlkm**`
    
    This partition is dedicated to storing vendor kernel modules. Storing vendor kernel modules in the `vendor_dlkm` partition (as opposed to the `vendor` partition) makes it possible to update kernel modules without updating the `vendor` partition.
    
- `**radio**`
    
    This partition contains the radio image and is needed only for devices that include a radio with radio-specific software in a dedicated partition.
    
- `**tos**`
    
    This partition stores the binary image of the Trusty OS and is used only if the device includes Trusty. For details, see [TOS Partitions](https://source.android.com/docs/core/architecture/bootloader/partitions/tos-partitions).

## References

- https://source.android.com/docs/core/architecture/partitions
