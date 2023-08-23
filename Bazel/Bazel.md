Bazel is a build system developed by google.
* Supports multiple languages.

1. WORKSPACE file
	* Identifies the Bazel workspace.
	* Lives at the root of the workspace.

2. Workspace
	The directory containing WORKSPACE file.

3. BUILD file
	* Specifies how to build different parts of the projects.
	* Contains rules.

4. Package
	* Directory containing BUILD file.
	* A package is a container of _targets_, which are defined in the package's `BUILD` file.

5. Targets
	* Buildable units.
	* Defined in BUILD file.
	* Identified by a label.

6. Rule target
	A target that is declared by instantiating a rule is called a rule target.

7. Rule

#### Rule

#### Action

#### Target

#### Rule Target

#### Attribute
Attributes are the parameters for a rule.

**For example,**
```python
cc_library(     name = "my_lib",     srcs = ["my_lib.cpp", "helper.cpp"],     hdrs = ["my_lib.h", "helper.h"],     visibility = ["//visibility:public"],     deps = ["//path/to:other_lib"], )
```

In this example, we're defining a C++ library target named **"my_lib"**. We specify the source files (**"srcs"**) and header files (**"hdrs"**) and dependencies (**"deps"**) associated with this library. All these are attributes used to define the rule.

#### Providers
Providers are pieces of information that a rule exposes to other rules that depend on it. 

When a rule generates an output, it can also attach providers to those outputs. These providers contain relevant information about the generated output, such as dependencies, attributes, and other data that might be needed by other rules consuming the output.

**For example,** if you have a rule that compiles C++ source files into object files, you can attach a provider containing information about the dependencies and compile options used for that source file. Other rules or actions that need to work with these object files can then access this provider to gather the necessary details.