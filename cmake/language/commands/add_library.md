# `add_library` command

Add a library to the project using the specified source files.

## Normal Library

Adds a library target called `<name>` to be built from the source files listed in the command invocation.

The `<name>` corresponds to the logical target name and must be globally unique within a project.

The actual file name of the library built is constructed based on conventions of the native platform (such as `lib<name>.a` or `<name>.lib`).

### Syntax

```cmake
add_library(<name> [STATIC | SHARED | MODULE [EXCLUDE_FROM_ALL] [<source>...])
```

### Parameters

> ###### `STATIC | SHARED | MODULE`
>
> `STATIC` libraries are archives of object files for use when linking other targets.
>
> `SHARED` libraries are linked dynamically and loaded at runtime.
>
> `MODULE` libraries are plugins that are not linked into other targets but may be loaded dynamically at runtime using dlopen-like functionality.
>
> If no type is given explicitly the type is `STATIC` or `SHARED` based on whether the current value of the variable [`BUILD_SHARED_LIBS`](https://cmake.org/cmake/help/latest/variable/BUILD_SHARED_LIBS.html#variable:BUILD_SHARED_LIBS "BUILD_SHARED_LIBS") is `ON`.

## Object Library

Creates an [Object Library](cmake/build-system/object-libraries).

### Syntax

```cmake
add_library(<name> OBJECT [<source>...])
```

## Interface Libary

Creates an [Interface Library](cmake/build-system/interface-libraries).

### Syntax

```cmake
add_library(<name> INTERFACE [<source>...] [EXCLUDE_FROM_ALL])
```

## Alias Library

Creates an [Alias Target](cmake/build-system/alias-targets), for`<target>`.

### Syntax

```cmake
add_library(<name> ALIAS <target>)
```

### Parameters

> ##### `<name>`
>
> The `<name>` does not appear in the generated build-system as a make target.

> ##### `<target>`
>
> The `<target>` itself cannot be an `ALIAS`.

## References

- https://cmake.org/cmake/help/latest/command/add_library.html