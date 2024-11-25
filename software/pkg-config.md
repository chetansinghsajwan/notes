# `pkg-config` tool

It is used to retrieve information about installed libraries in the system. It is typically used to compile and link against other libraries.

It is developed by [freedesktop](freedesktop).

It is a free software, licensed under the [GPL](software/licenses/gpl2) version 2.

For example,

```bash
cc program.c 'pkg-config --cflags --libs gnomeui'
```

## How it works?

It retrieves information about other packages from special metadata files. These files are named after the package, with the extension `.pc`.

It looks for these files in specific locations.

## References

- https://www.freedesktop.org/wiki/Software/pkg-config/
- https://linux.die.net/man/1/pkg-config
- https://people.freedesktop.org/~dbn/pkg-config-guide.html
- https://www.wikiwand.com/en/articles/Pkg-config
