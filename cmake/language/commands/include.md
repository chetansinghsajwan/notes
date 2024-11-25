# `include` command

Load and run code from a cmake module.

###### Syntax

```cmake

include(<file|module> [OPTIONAL] [RESULT_VARIABLE <var>] [NO_POLICY_SCOPE])

```

#### Parameters

###### `<file|module>`

Includes the code from the file specified.

If a module is specified instead of a file, the file with name `<module>.cmake` is searched first in [`cmake_module_path`](cmake-language/variables/cmake-module-path), then in the CMake module directory.

###### `OPTIONAL` #optional 

No error is raised if the file does not exist.

###### `RESULT_VARIABLE` #optional 

The variable `<var>` will be set to the full filename which has been included or `NOTFOUND` if it failed.

###### `NO_POLICY_SCOPE` #optional 

Doesn't add new scope for policies.

## References