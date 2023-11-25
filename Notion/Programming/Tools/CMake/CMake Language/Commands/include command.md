Load and run CMake code from a file or module.

###### Syntax

```cmake

include(<file|module> [OPTIONAL] [RESULT_VARIABLE <var>] [NO_POLICY_SCOPE])

```

#### Parameters

###### `OPTIONAL` #optional 

No error is raised if the file does not exist.

###### `RESULT_VARIABLE` #optional 

The variable `<var>` will be set to the full filename which has been included or `NOTFOUND` if it failed.

###### `NO_POLICY_SCOPE` #optional 

Doesn't add new scope for policies.