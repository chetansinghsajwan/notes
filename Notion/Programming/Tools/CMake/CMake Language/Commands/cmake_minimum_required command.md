Sets the minimum required version of cmake for a project.
**Syntax**
```
cmake_minimum_required(VERSION <min>[...<policy_max>] [FATAL_ERROR])
```
**Parameters**
**`min`**
If the running version of CMake is lower than the `<min>` required version it will stop processing the project and report an error.
**`policy_max`** **(optional)**
Version passed to [[cmake_policy command]] as `<max>`.
This command will set the value of the [[cmake_minimum_required_version variable]] variable to `<min>`.
The `cmake_minimum_required(VERSION)` command implicitly invokes the [[cmake_policy command]]`)`.

> [!important]  
> NoteCall the cmake_minimum_required() command at the beginning of the top-level CMakeLists.txt file even before calling the [[project command]] command.It is important to establish version and policy settings before invoking other commands whose behavior they may affect.  
  
> [!important]  
> Note  
  
Calling cmake_minimum_required() inside a [[function command]] limits some effects to the function scope when invoked.ExampleThe [[cmake_minimum_required_version variable]] variable won't be set in the calling scope.Functions do not introduce their own policy scope though, so policy settings of the caller will be affected. Due to this mix of things that do and do not affect the calling scope, calling cmake_minimum_required() inside a function is generally discouraged.