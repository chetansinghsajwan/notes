A _kit_ defines project-agnostic and configuration-agnostic info about how to build code.
- They are vscode-cmake-tools extension specific.
A kit can include:
- A set of compilers: these are locked at specific versions so that you can switch your compiler version quickly and easily.
- A Visual Studio installation: building for Visual Studio involves more than just finding the necessary compiler executable. Visual C++ requires certain environment variables to be set to tell it how to find and link to the Visual C++ toolchain headers and libraries.
- A toolchain file: The low-level way to instruct CMake how to compile and link for a target. CMake Tools handles toolchain files using kits.
  
# References

> [!info]  
>  
> [https://github.com/microsoft/vscode-cmake-tools/blob/main/docs/kits.md](https://github.com/microsoft/vscode-cmake-tools/blob/main/docs/kits.md)