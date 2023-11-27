**Dev Drive** is a new form of storage volume available to improve performance for key developer workloads.
## How Dev Drive works
The **Dev Drive** utilizes [[refs]] enabling you to initialize a storage volume specifically for development workloads, providing faster performance, and customizable settings that are optimized for development scenarios.
## Performance
![[/Untitled 10.png|Untitled 10.png]]
## Security
## **What should I put on my Dev Drive?**
Dev Drive is intended for:
- Source code repositories and project files
- Package caches
- Build output and intermediate files
Dev Drive is **not** intended to store developer tools, such as:
- Visual Studio
- MSBuild
- .NET SDK
- Windows SDK, etc.
## Requirements
- Windows 11, Build \#10.0.22621.2338 or later (Check for Windows updates)
- Recommend 16gb memory (minimum of 8gb)
- Minimum 50gb free disk space
- Dev Drives are available on all Windows SKU versions.
## Creating Dev Drive
Dev Drive can be created using [[virtual-hard-disk]] or a new volume.
- You can create multiple dev drives.
## **Deleting Dev Drive**
You can delete a dev drive just like deleting any other volume.
## **Storing package cache on Dev Drive**
A package cache is the global folder location used by applications to store files for installed software. These source files are needed when you want to update, uninstall, or repair the installed software. Visual Studio is one such application that stores a large portion of its data in the Package Cache.

> [!info] Set up a Dev Drive on Windows 11  
> Learn about the new Dev Drive storage available to improve file system performance for development scenarios using the ReFS volume format, including how to set it up, designate trust to use performance mode for Microsoft Defender Antivirus, customized filters, and FAQs.  
> [https://learn.microsoft.com/en-us/windows/dev-drive/#storing-package-cache-on-dev-drive](https://learn.microsoft.com/en-us/windows/dev-drive/#storing-package-cache-on-dev-drive)  
# References

> [!info] Set up a Dev Drive on Windows 11  
> Learn about the new Dev Drive storage available to improve file system performance for development scenarios using the ReFS volume format, including how to set it up, designate trust to use performance mode for Microsoft Defender Antivirus, customized filters, and FAQs.  
> [https://learn.microsoft.com/en-us/windows/dev-drive/](https://learn.microsoft.com/en-us/windows/dev-drive/)  

> [!info] Dev Drive: Performance, Security and Control for Developers  
> We are excited to introduce Dev Drive!  
> [https://blogs.windows.com/windowsdeveloper/2023/06/01/dev-drive-performance-security-and-control-for-developers/](https://blogs.windows.com/windowsdeveloper/2023/06/01/dev-drive-performance-security-and-control-for-developers/)