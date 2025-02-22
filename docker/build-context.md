# Docker Build Context

The build context is the set of files that your build can access. The positional argument that you pass to the build command specifies the context that you want to use for the build:

```console
 docker build [OPTIONS] PATH | URL | -
                         ^^^^^^^^^^^^^^
```

You can pass any of the following inputs as the context for a build:

- The relative or absolute path to a local directory
- A remote URL of a Git repository, tarball, or plain-text file
- A plain-text file or tarball piped to the `docker build` command through standard input

Build contexts have been divided into following types:

- [Local Context](/docker/build-context/)
- Remote Context
- Empty Context
- Named Context

## References

- https://docs.docker.com/build/concepts/context/
