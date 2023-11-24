---
URL: https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html
---
==3 August 2022==
==by JohT==
==Looking for new versions of libraries and updating them can be a tedious task, especially if you have to deal with multiple dependencies and many repositories.== [==Renovate==](https://github.com/renovatebot/renovate) ==is a tool that can scan your dependencies and update them to the latest version. But can this also be utilized in a C++ project with== [==CPM.cmake==](https://github.com/cpm-cmake/CPM.cmake)==?==
### ==Table of Contents==
1. [==Prerequisites==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html#prerequisites)
2. [==External Dependencies==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html#external-dependencies)
    1. [==What is CPM.cmake [2]?==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html\#what-is-cpmcmake-2)
    2. [==Why not just use Git Submodules [11]?==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html\#why-not-just-use-git-submodules-11)
    3. [==How to use CPM.cmake?==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html#how-to-use-cpmcmake)
    4. [==Why CPM.cmake?==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html#why-cpmcmake)
    5. [==CPM Package Lock [15]==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html\#cpm-package-lock-15)
3. [==Version Updates==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html#version-updates)
    1. [==What is Renovate [1]?==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html\#what-is-renovate-1)
    2. [==How to use Renovate?==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html#how-to-use-renovate)
4. [==How to use Renovate with CPM.cmake?==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html#how-to-use-renovate-with-cpmcmake)
    1. [==Renovate Regex Manager for CPM Package Lock VERSION==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html#renovate-regex-manager-for-cpm-package-lock-version)
    2. [==Renovate Regex Manager for CPM Package Lock GIT_TAG==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html#renovate-regex-manager-for-cpm-package-lock-git_tag)
    3. [==Renovate Regex Manager for CPM Package Lock GIT_TAG commit SHA==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html#renovate-regex-manager-for-cpm-package-lock-git_tag-commit-sha)
    4. [==Renovate Regex Manager for GitHub download links in CMake==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html#renovate-regex-manager-for-github-download-links-in-cmake)
    5. [==Renovate Regex Manager for GitHub downloads with a version variable in CMake==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html#renovate-regex-manager-for-github-downloads-with-a-version-variable-in-cmake)
    6. [==Real world example==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html#real-world-example)
5. [==Summary==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html#summary)
6. [==References==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html#references)
## ==Prerequisites==
- ==C++ project with== [==CMake [3]==](https://cmake.org/) ==as build tool.==
## ==External Dependencies==
### ==What is== [==CPM.cmake [2]==](https://github.com/cpm-cmake/CPM.cmake)==?==
==cpm is …==
==CMake’s missing package manager. A small CMake script for setup-free, cross-platform, reproducible dependency management.==
==A key difference to other package managers is:==
==Any downloadable project or resource can be added as a version-controlled dependency though CPM.==
### ==Why not just use== [==Git Submodules [11]==](https://git-scm.com/book/en/v2/Git-Tools-Submodules)==?==
==A common practice to include external sources in C++ project is to use== [==Git Submodules==](https://git-scm.com/book/en/v2/Git-Tools-Submodules)==. This is a good solution for many projects:==
- ==It doesn’t introduce any additional dependencies==
- ==Continuous Integration is easy to setup.==
- ==The build is reproducible since submodules are tied to specific Git commits.==
==But there are also some drawbacks:==
- ==It isn’t possible to use Git version tags directly (only commit SHA’s).==
- ==It can’t be used to download already built packages.==
- ==It can’t be used for sources that are not available in Git.==
==In the end it is a choice of what suits the project best.== [==Automated Dependency Updates for Git Submodules [12]==](https://docs.renovatebot.com/modules/manager/git-submodules) ==goes further into details on how to setup automatic update for Git Submodules. Otherwise continue reading if you are interested in what== [==CPM.cmake==](https://github.com/cpm-cmake/CPM.cmake) ==can do.==
### ==How to use== [==CPM.cmake==](https://github.com/cpm-cmake/CPM.cmake)==?==
==These few lines in== [==CMake==](https://cmake.org/)==’s== ==`CMakeLists.txt`== ==are all you need to setup== [==CPM.cmake==](https://github.com/cpm-cmake/CPM.cmake)==:==
```
set(CPM_DOWNLOAD_VERSION 0.27.2) 
set(CPM_DOWNLOAD_LOCATION "${CMAKE_BINARY_DIR}/cmake/CPM_${CPM_DOWNLOAD_VERSION}.cmake")
if(NOT (EXISTS ${CPM_DOWNLOAD_LOCATION}))
    message(STATUS "Downloading CPM.cmake")
    file(DOWNLOAD https://github.com/TheLartians/CPM.cmake/releases/download/v${CPM_DOWNLOAD_VERSION}/CPM.cmake ${CPM_DOWNLOAD_LOCATION})
endif()
include(${CPM_DOWNLOAD_LOCATION})
```
==As an example, adding Catch2 for unit testing looks like this (shorthand syntax):==
```
CPMAddPackage("gh:catchorg/Catch2@3.1.0")
```
==For more information have a look at this article:== [==CPM: An Awesome Dependency Manager for C++ with CMake [13]==](https://medium.com/swlh/cpm-an-awesome-dependency-manager-for-c-with-cmake-3c53f4376766)
### ==Why== [==CPM.cmake==](https://github.com/cpm-cmake/CPM.cmake)==?==
[==CPM.cmake==](https://github.com/cpm-cmake/CPM.cmake) ==can be used like== [==Git Submodules==](https://git-scm.com/book/en/v2/Git-Tools-Submodules) ==to include sources from other Git Repositories. It can furthermore be used to download already built packages and download sources that are not available in Git. It can work with git tags and commit references.== [==CPM.cmake==](https://github.com/cpm-cmake/CPM.cmake) ==wraps== [==CMake’s FetchContent [14]==](https://cmake.org/cmake/help/latest/module/FetchContent.html) ==to simplify downloading external dependencies. It doesn’t depend on a centralized package repository nor does it require any metadata to support== [==CPM.cmake==](https://github.com/cpm-cmake/CPM.cmake)==.==
### [==CPM Package Lock [15]==](https://github.com/cpm-cmake/CPM.cmake/wiki/Package-lock)
[==CPM.cmake==](https://github.com/cpm-cmake/CPM.cmake) ==provides a way to list all dependencies in one== ==`package-lock.cmake`== ==file. These can then easily be referenced in== ==`CMakeLists.txt`== ==by their name where needed. This mimics other package managers and has several advantages. It is easier to read, simplifies the process of updating dependencies and integrates much better with other tools.==
- ==Add== ==`CPMUsePackageLock`== ==right after including== [==CPM.cmake==](https://github.com/cpm-cmake/CPM.cmake)==:==
    
    ```
    include(cmake/CPM.cmake)
    CPMUsePackageLock(package-lock.cmake)
    ```
    
- ==Build the utility target:==
    
    ```
    cmake -H. -Bbuild
    cmake --build build --target cpm-update-package-lock
    ```
    
==The resulting== ==`package-lock.cmake`== ==may then look for example like this:==
```
# CPM Package Lock
# This file should be committed to version control
# Catch2
CPMDeclarePackage(Catch2
  VERSION 3.1.0
  GITHUB_REPOSITORY catchorg/Catch2
  EXCLUDE_FROM_ALL YES
)
```
==The library is then referenced by name within== ==`CMakeLists.txt`== ==like this:==
## ==Version Updates==
==It is a tedious and time consuming task to update dependencies to their latest version manually. Tools like== [==Renovate==](https://github.com/renovatebot/renovate) ==automate this process by scanning your dependencies and updating them to the latest version. Staying up-to-date is essential when it comes to security updates and bug fixes. It is also easier to continuously update in small steps. Last but not least an update might come in handy when there are useful new features.==
### ==What is== [==Renovate [1]==](https://github.com/renovatebot/renovate)==?==
==Renovate is a …==
==universal dependency update tool that fits into your workflows==
==… and let you …==
==get automated Pull Requests to update your dependencies==
### ==How to use== [==Renovate==](https://github.com/renovatebot/renovate)==?==
==If your repository is on== [==GitHub [6]==](https://github.com/)==,== [==Renovate GitHub App [4]==](https://github.com/apps/renovate) ==is free to install.== [==Installing and onboarding Renovate into repositories [5]==](https://docs.renovatebot.com/getting-started/installing-onboarding) ==gives you more information about that.==
==The configuration for Renovate is usually found in the file== ==`renovate.json`====. For further options see== [==Renovate Configuration Options [7]==](https://docs.renovatebot.com/configuration-options)==. The most basic configuration looks like this:==
```
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "config:base"
  ]
}
```
## ==How to use== [==Renovate==](https://github.com/renovatebot/renovate) ==with== [==CPM.cmake==](https://github.com/cpm-cmake/CPM.cmake)==?==
[==CPM.cmake [2]==](https://github.com/cpm-cmake/CPM.cmake) ==isn’t supported by== [==Renovate==](https://github.com/renovatebot/renovate) ==yet (August 2022). Fortunately, there is a workaround for that. As described in== [==Custom Manager Support using Regex [8]==](https://docs.renovatebot.com/modules/manager/regex)==,== [==Regular Expressions [10]==](https://en.wikipedia.org/wiki/Regular_expression) ==can be used to identify dependencies and then find and replace their versions.==
==ⓘ Note that it is recommended to use a tool like== [==regex101.com==](https://regex101.com/) ==to get immediate feedback when developing== [==Regular Expressions==](https://en.wikipedia.org/wiki/Regular_expression)==.==
### [==Renovate Regex Manager==](https://docs.renovatebot.com/modules/manager/regex) ==for== [==CPM Package Lock==](https://github.com/cpm-cmake/CPM.cmake/wiki/Package-lock) ==VERSION==
==The following== ==`package-lock.cmake`== ==entry shows a dependency to a== [==GitHub==](https://github.com/) ==repository with a specific version:==
```
CPMDeclarePackage(Catch2
  VERSION 3.1.0
  GITHUB_REPOSITORY catchorg/Catch2
  EXCLUDE_FROM_ALL YES
)
```
==Use this== ==`renovate.json`== ==to update the dependency’s version with a pull request whenever there is a new release:==
```
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "config:base",
  ],
  "regexManagers": [
    {
      "fileMatch": ["(^|/)package-lock\\.cmake$"],
      "matchStrings": [
        "CPMDeclarePackage\\s*\\(\\s*\\w+\\s*.*VERSION\\s+\"?(?<currentValue>.*?)\"?\\s+GITHUB_REPOSITORY\\s+\"?(?<depName>.*?)\"?[\\s\\)]",
        "CPMDeclarePackage\\s*\\(\\s*\\w+\\s*.*GITHUB_REPOSITORY\\s+\"?(?<depName>.*?)\"?\\s+VERSION\\s+\"?(?<currentValue>.*?)\"?[\\s\\)]"
      ],
      "datasourceTemplate": "github-releases",
      "extractVersionTemplate": "^v?(?<version>.*?)$"
    }
  ]
}
```
==ⓘ Note that there are two similar== [==Regular Expressions==](https://en.wikipedia.org/wiki/Regular_expression) ==to be independent from the order of== ==`VERSION`== ==and== ==`GITHUB_REPOSITORY`====.==
==ⓘ Note that== [==CMake==](https://cmake.org/) ==values can be given with or without surrounding double quotes. This is reflected within the== [==Regular Expression==](https://en.wikipedia.org/wiki/Regular_expression) ==by== ==`\"?`====.==
==ⓘ Note that== ==`extractVersionTemplate`== ==is necessary to extract the version including the “v” prefix from the GitHub release tag whereas it is given without the prefix.==
==ⓘ Note that it is also possible to observe all tags and not only those published as a release using== ==`"datasourceTemplate": "github-tags"`====.==
### [==Renovate Regex Manager==](https://docs.renovatebot.com/modules/manager/regex) ==for== [==CPM Package Lock==](https://github.com/cpm-cmake/CPM.cmake/wiki/Package-lock) ==GIT_TAG==
==The following== ==`package-lock.cmake`== ==entry shows a dependency to a== [==GitHub==](https://github.com/) ==repository with a specific git tag (without the “v” prefix):==
```
CPMDeclarePackage(JUCE
  GIT_TAG 7.0.1
  GITHUB_REPOSITORY juce-framework/JUCE
  EXCLUDE_FROM_ALL YES
)
```
==Use this regexManager in== ==`renovate.json`== ==to update the dependency’s git tag with a pull request whenever there is a new release:==
```
{
  "fileMatch": ["(^|/)package-lock\\.cmake$"],
  "matchStrings": [
    "CPMDeclarePackage\\s*\\(\\s*\\w+\\s*.*GIT_TAG\\s+\"?(?<currentValue>.*?[^0-9a-f\\s]+.*|.{1,4}?)\"?\\s+GITHUB_REPOSITORY\\s+\"?(?<depName>.*?)\"?\\s+",
    "CPMDeclarePackage\\s*\\(\\s*\\w+\\s*.*GITHUB_REPOSITORY\\s+\"?(?<depName>.*?[^0-9a-f\\s]+.*|.{1,4}?)\"?\\s+GIT_TAG\\s+\"?(?<currentValue>.*?)\"?\\s+"
  ],
  "datasourceTemplate": "github-releases"
}
```
==ⓘ Note that the GIT_TAG value needs to contain at least one character that distinguishes it from a Git SHA. Also values with less than 4 characters are considered Non-SHA. This is reflected within the== [==Regular Expression==](https://en.wikipedia.org/wiki/Regular_expression) ==by== ==`.*?[^0-9a-f\\s]+.*|.{1,4}`====. GIT_TAG supports both tags and commits. They need to be distinguished because they need to be updated differently.==
### [==Renovate Regex Manager==](https://docs.renovatebot.com/modules/manager/regex) ==for== [==CPM Package Lock==](https://github.com/cpm-cmake/CPM.cmake/wiki/Package-lock) ==GIT_TAG commit SHA==
==The following== ==`package-lock.cmake`== ==entry shows a dependency to a== [==GitHub==](https://github.com/) ==repository with a specific git commit:==
```
CPMDeclarePackage(span
 GIT_TAG 836dc6a0efd9849cb194e88e4aa2387436bb079b
 GITHUB_REPOSITORY tcbrindle/span
 EXCLUDE_FROM_ALL YES
)
```
==Use this regexManager in== ==`renovate.json`== ==to update the dependency’s git SHA with a pull request whenever there is a new commit in the “master” branch:==
```
{
  "fileMatch": ["(^|/)package-lock\\.cmake$"],
  "matchStrings": [
    "CPMDeclarePackage\\s*\\(\\s*\\w+\\s*.*GIT_TAG\\s+\"?(?<currentDigest>[0-9a-f]{5,40}?)\"?\\s+GITHUB_REPOSITORY\\s+?\"?(?<depName>.*?)\"?\\s+",
    "CPMDeclarePackage\\s*\\(\\s*\\w+\\s*.*GITHUB_REPOSITORY\\s+\"?(?<depName>[0-9a-f]{5,40}?)\"?\\s+GIT_TAG\\s+?\"?(?<currentDigest>.*?)\"?\\s+"
  ],
  "datasourceTemplate": "git-refs",
  "depNameTemplate": "https://github.com/{{{depName}}}.git",
  "currentValueTemplate": "master"
}
```
==ⓘ Note that the GIT_TAG value needs to be a Git SHA. This is reflected within the== [==Regular Expression==](https://en.wikipedia.org/wiki/Regular_expression) ==by== ==`[0-9a-f]{5,40}`====. GIT_TAG supports both tags and commits. They need to be distinguished because they need to be updated differently.==
==ⓘ Note that== ==`currentDigest`== ==is used instead of== ==`currentValue`== ==to reflect the fact that it is a unique SHA and not a version (that could contain e.g. wildcards).== ==`currentValue`== ==won’t work here.==
==ⓘ Note that== ==`depNameTemplate`== ==is used to construct the dependency’s Git URL since== [==Renovate==](https://github.com/renovatebot/renovate) ==doesn’t support a dataSource like “github-refs” yet. Nevertheless this can be very useful when a custom Git URL is needed.==
==ⓘ Note that== ==`currentValueTemplate`== ==needs to be changed to “main” if this is the branch you want to observe.==
### [==Renovate Regex Manager==](https://docs.renovatebot.com/modules/manager/regex) ==for== [==GitHub==](https://github.com/) ==download links in== [==CMake==](https://cmake.org/)
==Before it can be used,== [==CPM.cmake==](https://github.com/cpm-cmake/CPM.cmake) ==needs to be downloaded. This can be done at build time with== [==CMake==](https://cmake.org/)==. The following== ==`CMakeLists.txt`== ==line shows a simple uncached solution to download== [==CPM.cmake==](https://github.com/cpm-cmake/CPM.cmake) ==from a specific== [==GitHub==](https://github.com/) ==release. A== [==cached solution with variable==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html#renovate-regex-manager-for-github-downloads-with-a-version-variable-in-cmake) ==is shown further below.==
```
file(DOWNLOAD https://github.com/cpm-cmake/CPM.cmake/releases/download/v0.35.4/CPM.cmake ${CMAKE_BINARY_DIR}/cmake/CPM.cmake)
```
==Use this regexManager in== ==`renovate.json`== ==to update== [==CPM.cmake==](https://github.com/cpm-cmake/CPM.cmake) ==with a pull request whenever there is a new release:==
```
{
  "fileMatch": ["(^|/)\\w+\\.cmake$", "(^|/)CMakeLists\\.cmake$"],
  "matchStrings": [
    "https:\\/\\/github\\.com\\/(?<depName>[^{}]*?)\\/releases\\/download\\/(?<currentValue>[^{}]*?)\\/"
  ],
  "datasourceTemplate": "github-releases"
}
```
==ⓘ Note that this== [==Regular Expression==](https://en.wikipedia.org/wiki/Regular_expression) ==can be used to update any== [==GitHub==](https://github.com/) ==release download link, not just== [==CPM.cmake==](https://github.com/cpm-cmake/CPM.cmake)==.==
==ⓘ Note that this doesn’t work when the version is taken from a== [==CMake==](https://cmake.org/) ==variable. This is assured within the== [==Regular Expression==](https://en.wikipedia.org/wiki/Regular_expression) ==with== ==`[^{}]*`====.==
### [==Renovate Regex Manager==](https://docs.renovatebot.com/modules/manager/regex) ==for== [==GitHub==](https://github.com/) ==downloads with a version variable in== [==CMake==](https://cmake.org/)
==To download== [==CPM.cmake==](https://github.com/cpm-cmake/CPM.cmake) ==with the version specified in a== [==CMake==](https://cmake.org/) ==variable as shown above in== [==How to use CPM.cmake?==](https://joht.github.io/johtizen/automation/2022/08/03/keep-your-cpp-dependencies-up-to-date.html#how-to-use-cpmcmake)==, there need to be some sort of convention to be able to update it with== [==Renovate==](https://joht.github.io/johtizen/automation/2022/08/03/Renovate)==.==
==The following regexManager in== ==`renovate.json`== ==shows one possible solution to update the version variable for the download with a pull request whenever there is a new release:==
```
{
  "fileMatch": ["(^|/)\\w+\\.cmake$", "(^|/)CMakeLists\\.cmake$"],
  "matchStrings": [
    "set\\s*\\(\\s*\\w+VERSION\\s+\"?(?<currentValue>[^v$][^${}]*?)\"?\\s*\\)[\\S\\s]*https:\\/\\/github.com\\/(?<depName>.*?)\\/releases\\/download\\/v\\$\\{\\w+VERSION\\}"
  ],
  "datasourceTemplate": "github-releases",
  "extractVersionTemplate": "^v?(?<version>.*?)$"
}
```
==ⓘ Note that the variable containing the version needs to have a name ending with “VERSION”. It also needs to be defined right before the download.==
==ⓘ Note that since== [==Regular Expression RE2==](https://github.com/google/re2/wiki/WhyRE2) ==doesn’t support backreferences, the name of the version variable is only checked to end with “VERSION”, it isn’t checked to be actual equal to its definition.==
### ==Real world example==
==The repository== [==speclet==](https://github.com/JohT/speclet) ==shows a real world example with all the above mentioned snippets in these files:==
- [==renovate.json==](https://github.com/JohT/speclet/blob/main/renovate.json)
- [==package-lock.cmake==](https://github.com/JohT/speclet/blob/main/package-lock.cmake)
- [==Environment.cmake==](https://github.com/JohT/speclet/blob/main/cmake/Environment.cmake)
## ==Summary==
==To answer the question of the introduction: C++ projects can of course also benefit from a modern tool like== [==Renovate==](https://github.com/renovatebot/renovate)==. Even if package managers like== [==CPM.cmake==](https://github.com/cpm-cmake/CPM.cmake) ==aren’t supported yet, automatic version updates can still be customized using== [==Renovate Regex Managers==](https://docs.renovatebot.com/modules/manager/regex)==.==
[==renovate.json==](https://github.com/JohT/speclet/blob/main/renovate.json) ==shows a real world example with the whole configuration. It relies on== [==package-lock.cmake==](https://github.com/JohT/speclet/blob/main/package-lock.cmake) ==containing the description of all dependencies.==
==It should also be possible to extend the configuration for projects that don’t use== [==CPM Package Lock==](https://github.com/cpm-cmake/CPM.cmake/wiki/Package-lock) ==as well as for those that use== [==CPM.cmake’s==](https://github.com/cpm-cmake/CPM.cmake) ==shorthand syntax in future.==
## ==References==
- [==[1] Renovate==](https://github.com/renovatebot/renovate)==  
    ====https://github.com/renovatebot/renovate==
- [==[2] CPM.cmake==](https://github.com/cpm-cmake/CPM.cmake)==  
    ====https://github.com/cpm-cmake/CPM.cmake==
- [==[3] CMake==](https://cmake.org/)==  
    ====https://cmake.org==
- [==[4] Renovate GitHub App==](https://github.com/apps/renovate)==  
    ====https://github.com/apps/renovate==
- [==[5] Installing and onboarding Renovate into repositories==](https://docs.renovatebot.com/getting-started/installing-onboarding)==  
    ====https://docs.renovatebot.com/getting-started/installing-onboarding==
- [==[6] GitHub==](https://github.com/)==  
    ====https://github.com==
- [==[7] Renovate Configuration Options==](https://docs.renovatebot.com/configuration-options)==  
    ====https://docs.renovatebot.com/configuration-options==
- [==[8] Renovate Custom Manager Support using Regex==](https://docs.renovatebot.com/modules/manager/regex)==  
    ====https://docs.renovatebot.com/modules/manager/regex==
- [==[10] Regular Expression==](https://en.wikipedia.org/wiki/Regular_expression)==  
    ====https://en.wikipedia.org/wiki/Regular_expression==
- [==[11] GitSubmodules==](https://git-scm.com/book/en/v2/Git-Tools-Submodules)==  
    ====https://git-scm.com/book/en/v2/Git-Tools-Submodules==
- [==[12] Automated Dependency Updates for Git Submodules==](https://docs.renovatebot.com/modules/manager/git-submodules)==  
    ====https://docs.renovatebot.com/modules/manager/git-submodules==
- [==[13] CPM: An Awesome Dependency Manager for C++ with CMake==](https://medium.com/swlh/cpm-an-awesome-dependency-manager-for-c-with-cmake-3c53f4376766)==  
    ====https://medium.com/swlh/cpm-an-awesome-dependency-manager-for-c-with-cmake-3c53f4376766==
- [==[14] CMake FetchContent==](https://cmake.org/cmake/help/latest/module/FetchContent.html)==  
    ====https://cmake.org/cmake/help/latest/module/FetchContent.html==
- [==[15] CPM Package Lock==](https://github.com/cpm-cmake/CPM.cmake/wiki/Package-lock)==  
    ====https://github.com/cpm-cmake/CPM.cmake/wiki/Package-lock==
- [==[16] Regular Expression RE2==](https://github.com/google/re2/wiki/WhyRE2)==  
    ====https://github.com/google/re2/wiki/WhyRE2==
==tags:== ==_renovate_== ==-== ==_cpm_== ==-== ==_dependency_== ==-== ==_version_==
==**Hint:**== ==If you want to reach out to me without leaving a comment below, open a new== [==discussion on GitHub==](https://github.com/JohT/johtizen/discussions)==.==