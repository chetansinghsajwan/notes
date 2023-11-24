---
URL: https://crascit.com/2016/01/31/enhanced-source-file-handling-with-target_sources/
---
**Updated December 2018:** _Parts of this article have been reworked to account for improvements made with the CMake 3.13.0 release. Key updates are noted within the article._
In all but trivial CMake projects, it is common to find targets built from a large number of source files. These files may be distributed across various subdirectories, which may themselves be nested multiple levels deep. In such projects, traditional approaches usually either list all source files at the top-most level or build up the list of source files in a variable and pass that to `add_library()`, `add_executable()`, etc. With CMake 3.1, a new command `target_sources()` was introduced which provides the missing piece among the various `target_...` commands. While the [CMake documentation](https://cmake.org/cmake/help/latest/command/target_sources.html) succintly describes what `target_sources()` does, it fails to highlight just how useful the new command is and why it promotes better CMake projects:
- It can lead to cleaner and more concise CMakeLists.txt project files.
- Dependency information can be specified closer to where the actual dependencies exist in the directory hierarchy.
- Source files gain the ability to become part of a target’s interface.
- Source files can be added to third party project targets without having to modify the third party project files.
## Life Before target_sources()
Typically, developers first learn CMake in a very simple manner, defining a target by listing the source files directly in the `add_executable()` or `add_library()` command itself. Eg:
```
add_executable(myApp src1.cpp src2.cpp)
```
When the number of source files grows large and they get distributed over a number of subdirectories, possibly nested to multiple levels, this quickly becomes unwieldly. It also results in having to repeat the directory structure, which reduces the benefit of structuring source files into directories in the first place.
The logical improvement many developers then make is to build up the list of source files in a variable as each subdirectory is pulled in via `include()`. Then after all the subdirectories have been included, `add_executable()` or `add_library()` is called, but this time passing just the variable instead of an explicit list of files. The top level CMakeLists.txt file then looks something like this:
```
# The name of the included file could be anything,
# it doesn't have to be called CMakeLists.txt
include(foo/CMakeLists.txt)
include(bar/CMakeLists.txt)
add_executable(myApp ${myApp_SOURCES})
```
with the subdirectory files structured something like this:
```
list(APPEND myApp_SOURCES
    ${CMAKE_CURRENT_LIST_DIR}/foo.cpp
    ${CMAKE_CURRENT_LIST_DIR}/foo_p.cpp
)
```
This allows each subdirectory to define just the sources it provides and to delegate any further nested subdirectories with another `include()`. It also keeps the top level CMakeLists.txt file quite small and the CMakeLists.txt in each subdirectory also tends to be reasonably uncomplicated and focussed just on the things in that directory.
As an alternative to explicitly building up a list of source files in a variable, some developers instead choose to let CMake find the source files and generate the contents of that variable automatically with the `file(GLOB_RECURSE ...)` command. While at first this may seem very attractive for its simplicity, this technique has a number of drawbacks and is [actively discouraged by the CMake documentation](https://cmake.org/cmake/help/latest/command/file.html). Nevertheless, it is often used by developers new to CMake until they experience first hand the problems the technique introduces.
## target_sources(): All The Advantages Without The Drawbacks
The down sides of the above approach may not be immediately obvious. One drawback is that the source files are built up in a variable and then that variable is passed to an `add_library()` or `add_executable()` call at the top level CMakeLists.txt file. Variables used in this way are not a particularly robust way to record the source file list. For example, if many targets are being built up throughout a directory heirarchy, then the number and naming of variables can get out of hand. This can be somewhat addressed by sticking to some kind of naming convention associated with the target a variable is used with, but this relies on all developers knowing and adhering to that convention. Furthermore, if a developer inadvertently tries to re-use a variable name in a deeper directory level, sources could end up being added to unintended targets. CMake won’t typically issue any sort of diagnostic message, since it won’t know you didn’t intend to do that.
But perhaps the bigger drawback of using variables is that it precludes having the CMake target defined when descending into the subdirectories. This in turn means that subdirectories cannot directly call `target_compile_definitions()`, `target_compile_options()`, `target_include_directories()` or `target_link_libraries()` either. In order to associate compiler flags, options, header search paths and other libraries to be linked, more variables have to be defined to pass this information back up to the top level. Extra care has to be taken to properly handle quoting when doing this too. If you want to take full advantage of the PUBLIC, PRIVATE and INTERFACE capabilities of these various `target_...` commands too, the number of variables required just for one target alone already starts getting a bit silly. You can imagine the explosion of variables if many targets are defined throughout your project’s directory structure!
_NOTE: The advice and examples below have been updated from the original article to account for new capabilities added in CMake 3.13.0_
An example should help to highlight why `target_sources()` leads to much more robust and concise CMakeLists.txt files. Let’s say we have a project with two subdirectories `foo` and `bar`. The top level CMakeLists.txt file can be as simple as this:
```
cmake_minimum_required(VERSION 3.13)
project(MyProj)
add_library(myLib "")
add_subdirectory(foo)
add_subdirectory(bar)
```
The empty quotes in the `add_library()` call are necessary because that command requires a list of source files, even if that list is empty. If there were sources to be added from this top level directory, they could be listed there.
Let’s now assume the source files in the `foo` subdirectory use features from some external third party library called `barry`. Don’t worry about where `barry` comes from or what it represents, that’s not important for this discussion. It might come from some other part of the project, or be found somewhere on the system (the example leaves out details for the latter case, but again, this isn’t the main focus of the article). Now, because the source files of `myLib` use things from `barry`, `myLib` has to link against the `barry` library. For the sake of discussion, let’s also assume we need to define a compiler symbol called `USE_BARRY` both when building `myLib` and also for any code that includes headers from `myLib`. Assuming a minimum CMake version of 3.13.0 or later, the CMakeLists.txt file within the `foo` subdirectory might then look something like this:
```
target_sources(myLib
    PRIVATE
        foo.cpp
        foo_p.cpp
        foo_p.h
    PUBLIC
        foo.h  # poor PUBLIC example, see discussion below for why
)
find_library(BARRY_LIB barry)
# This call requires CMake 3.13 or later, see next section
target_link_libraries(myLib PUBLIC ${BARRY_LIB})
target_compile_definitions(myLib PUBLIC USE_BARRY)
target_include_directories(myLib PUBLIC ${CMAKE_CURRENT_LIST_DIR})
```
In the above example, note that .h header files were specified as sources too, not just the .cpp implementation files. Headers listed as sources don’t get compiled directly on their own, but the effect of adding them is for the benefit of IDE generators like Visual Studio, Xcode, Qt Creator, etc. This causes those headers to be listed in the project’s file list within the IDE, even if no source file refers to it via `\#include`. This can make those headers easier to find during development and potentially aid things like refactoring functionality, etc. **UPDATE:** With CMake 3.23 or later, [file sets](https://cmake.org/cmake/help/latest/command/target_sources.html#file-sets) are a better way to achieve this.
The `PRIVATE` and `PUBLIC` keywords specify where those corresponding sources should be used. `PRIVATE` simply means those sources should only be added to `myLib`, whereas `PUBLIC` means those sources should be added to `myLib` and to any target that links to `myLib`. An `INTERFACE` keyword can be used for sources that should not be added to `myLib` but should be added to anything that links to `myLib`. In practice, sources will almost always be `PRIVATE`, since they shouldn’t generally be added to anything that links against the target. Header-only interface libraries are one exception because sources can only be added as `INTERFACE` for interface libraries. Do not confuse the `PRIVATE` , `PUBLIC` and `INTERFACE` keywords with whether a header is part of the public API for the library or not, the keywords are specifically for controlling which target(s) the sources are added to in this case. There are also some less common cases where some files (eg resources, images, data files) may need to be compiled directly into targets linking against a library for them to be found at runtime. Listing such sources as `PUBLIC` or `INTERFACE` can help address such situations. Note though that installing a non-private source can be somewhat problematic (we will return to this topic further below).
The same meaning for `PRIVATE`, `PUBLIC` and `INTERFACE` apply to the other `target_...()` commands too, although it is more common to see things as non-private. The above example shows how easy it is to specify that `myLib` and any target that links to it also needs to link to the `barry` library. Similarly, with just that one `target_compile_definitions()` call, both `myLib` and anything linking against it will have the `USE_BARRY` symbol defined. Lastly, the `target_include_directories()` command adds the `foo` subdirectory to the header search path for both `myLib` and anything linking to it. Therefore, any other source file in another directory that needs to `\#include` the `foo.h` header will also be able to find it.
To illustrate just how powerful these `target_...()` commands are, let’s consider what the CMakeLists.txt file for the `bar` subdirectory might look like. In this case, let’s just assume `bar` needs to add a few sources files and that some of `bar`‘s sources or headers will include `foo.h`.
```
target_sources(myLib
    PRIVATE
        bar.cpp
        bar.h
        gumby.cpp
        gumby.h
)
```
Note the complete absence of anything other than simply listing the source files. All the work was done in the `foo` directory already, so there’s nothing left for us to do here. This highlights one of the biggest advantages of using `target_sources()`, namely that dependencies can be listed right where they are most relevant and all other directories don’t need to care. This localisation of dependency details leads to much more robust and more concise CMakeLists.txt files throughout a project. Without `target_sources()`, we would not be able to use `target_compile_definitions()`, `target_compile_options()`, `target_include_directories()` or `target_link_libraries()` in this way because the CMake target `myLib` would not be defined when we descend into each subdirectory.
### Supporting CMake 3.12 And Earlier
The above comments about using CMake 3.13.0 or later relate to restrictions that were removed in that release. Prior to 3.13.0, `target_link_libraries()` could only be called on a target that was created in the same directory scope. This means that in the example for the `foo` subdirectory, the `target_link_libraries(myLib ...)` call would cause an error with CMake 3.12 or earlier because the `myLb` target was created in the parent scope. None of the other `target_...()` commands have ever had this restriction, only `target_link_libraries()`. We will address this point shortly.
Another change in CMake 3.13.0 relates to how `target_sources()` interprets relative paths to source files. In CMake 3.12 or earlier, relative paths were treated as being relative to the target to which sources were being added. This was unintuitive, so CMake 3.13.0 changed the default behavior to treating relative paths as being relative to the current source directory instead. If the project sets 3.13.0 as its minimum CMake version requirement, it automatically gets the new behavior by default.
For projects that need to support CMake 3.12 or earlier, they can use absolute paths to source files to avoid the change in behavior and to avoid any policy warnings. For example:
```
target_sources(myLib
    PRIVATE
        ${CMAKE_CURRENT_LIST_DIR}/foo.cpp
        ${CMAKE_CURRENT_LIST_DIR}/foo_p.cpp
        ${CMAKE_CURRENT_LIST_DIR}/foo_p.h
    PUBLIC
        ${CMAKE_CURRENT_LIST_DIR}/foo.h
)
```
This is less convenient and less readable, so it may be helpful to define a helper function to give something close to the new behavior, but which also works for earlier CMake versions. CMake has a strong requirement on preserving backward compatibility, so the change in behavior of how relative paths are treated is guarded by a policy, [CMP0076](https://cmake.org/cmake/help/latest/policy/CMP0076.html). We can take advantage of that in the helper function:
```
# NOTE: This helper function assumes no generator expressions are used
#       for the source files
function(target_sources_local target)
  if(POLICY CMP0076)
    # New behavior is available, so just forward to it by ensuring
    # that we have the policy set to request the new behavior, but
    # don't change the policy setting for the calling scope
    cmake_policy(PUSH)
    cmake_policy(SET CMP0076 NEW)
    target_sources(${target} ${ARGN})
    cmake_policy(POP)
    return()
  endif()
  # Must be using CMake 3.12 or earlier, so simulate the new behavior
  unset(_srcList)
  get_target_property(_targetSourceDir ${target} SOURCE_DIR)
  foreach(src ${ARGN})
    if(NOT src STREQUAL "PRIVATE" AND
       NOT src STREQUAL "PUBLIC" AND
       NOT src STREQUAL "INTERFACE" AND
       NOT IS_ABSOLUTE "${src}")
      # Relative path to source, prepend relative to where target was defined
      file(RELATIVE_PATH src "${_targetSourceDir}" "${CMAKE_CURRENT_LIST_DIR}/${src}")
    endif()
    list(APPEND _srcList ${src})
  endforeach()
  target_sources(${target} ${_srcList})
endfunction()
```
Now we can call the above helper function just like the builtin command and get the CMake 3.13 behavior even with CMake 3.12 or earlier:
```
target_sources_local(myLib
    PRIVATE
        foo.cpp
        foo_p.cpp
        foo_p.h
    PUBLIC
        foo.h
)
```
When using CMake 3.12 or earlier, working around the restriction with `target_link_libraries()` is harder. The choices are either to move the `target_link_libraries()` call up to the same directory in which the target is defined, or avoid creating new directory scopes by using `include()` instead of `add_subdirectory()`. The second of these options would only require changing the top level CMakeLists.txt file to something like the following (which is the original method suggested by this article before it was updated for CMake 3.13.0):
```
cmake_minimum_required(VERSION 3.1)
project(MyProj)
add_library(myLib "")
# Using include() avoids creating a new directory scope, so these directories
# are able to call target_link_libraries(myLib ...)
include(foo/CMakeLists.txt)
include(bar/CMakeLists.txt)
```
The `target_sources_local()` helper function is defined in such a way that it will work inside `foo` or any other directory, regardless of whether we use `add_subdirectory()` or `include()`.
Most developers find `add_subdirectory()` more natural and it does tend to give more intuitive handling of variables like `CMAKE_CURRENT_SOURCE_DIR`, `CMAKE_CURRENT_BINARY_DIR`, etc. Therefore, if the subdirectories don’t need to call `target_link_libraries()`, prefer to use the `add_subdirectory()` approach rather than the above `include()` workaround.
### Complications For Installing
Specifying `PRIVATE` sources is relatively easy and has few difficulties. The location of each source file is clear and only needs to be considered within the build. Any `PUBLIC` or `INTERFACE` sources give rise to additional factors which must be considered. Within the build, paths to non-private sources are handled just like private ones, but when the project is installed, non-private paths have to make sense not only in the project’s own build, but also in the build of anything consuming the installed project. The paths used for the project’s own build will be specific to the machine on which that build is performed and to the directory in which it is built. Once installed, those directories will likely not be accessible, so the paths would need to be replaced with something else that makes sense for the installed set of files. This is made possible using the `BUILD_INTERFACE` and `INSTALL_INTERFACE` generator expressions. The following example shows how to provide different paths for the same file in the two different contexts:
```
target_sources(myHeaderOnly
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/algo.h>
        $<INSTALL_INTERFACE:include/algo.h>
)
install(FILES algo.h DESTINATION include)
```
While the above solves the build/install differences, it also has the drawback of requiring us to spell out every `BUILD_INTERFACE` path as absolute once again. We can no longer use our `target_sources_local()` helper function that we defined earlier. This is the cost of supporting the installation of non-private source files. Projects should weigh up the loss of convenience and added complexity against the expected benefits to be gained by making sources non-private.
## Key Points
Taking a step back, what `target_sources()` is doing for us is to remove the need for variables by allowing us to use CMake targets directly. This gives us these key advantages:
- It allows the CMake target to be defined early, which in turn enables calling the various other `target_...` commands in any of the subdirectories pulled in with `add_subdirectory()` after the target is defined.
- Subdirectories cannot inadvertently add sources to the wrong targets.
- Dependency information can be fully and robustly defined at the point where those dependencies are introduced. The `PRIVATE`, `PUBLIC` and `INTERFACE` keywords give precise control over the nature of those dependencies and they also promote better integration with IDE environments able to take advantage of this information.
The following points should also be kept in mind:
- Non-private sources require much more verbose and less convenient syntax, so consider whether the gains from making them non-private are worth it.
- If you need to support CMake 3.12 or older, you will need to either pull up any `target_link_libraries()` calls to the same directory as the target they operate on, or else use `include()` rather than `add_subdirectory()` to avoid introducing a new directory scope. Prefer the former where possible, since it is likely to be more intuitive for developers.
The `target_sources()` command also has one unique advantage over and above anything that a variable-based approach possesses. It allows additional sources to be added to targets regardless of where they were defined (except for imported targets). This is especially useful when code from an external project is being incorporated into a build with `add_subdirectory()` or `include()` (see [an earlier article](https://crascit.com/2015/07/25/cmake-gtest/) showing how to incorporate GoogleTest directly into your main build with these commands). This can be used to add headers, images, etc. from the external project without affecting the way the target is built. For the really adventurous, you could even potentially use this technique to add your own implementation for a [weak symbol](https://en.wikipedia.org/wiki/Weak_symbol) such that your implementation overrides the one that the external project’s target would normally use. This may prove useful during testing or to provide a more efficient implementation of a specific function, etc.