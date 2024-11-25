# `derivation` function

It takes as input an attribute set, the attributes of which specify the inputs to the process. It outputs an attribute set, and produces a [store derivation](programming/tools/nix/store-derivation) as a side effect of evaluation.

## Parameters

---

#### `name`
^attr-name

- type: [`string`](nix/language/data-types/string.md)
- required: yes

Name of the derivation.

It is added to the store path of the corresponding store derivation 
as well as to its [output paths](https://nixos.org/manual/nix/stable/glossary#gloss-output-path).

> [!example]
> 
> For `name = "hello"`, the store derivation's path will be `/nix/store/<hash>-hello.drv` and the [output](#^param-outputs) paths will be of the form `/nix/store/<hash>-hello[-<output>]`

---

#### `system`
^attr-system

- type: [`string`](nix/language/data-types/string.md)
- required: yes

The system type on which the [`builder`](#^param-builder) executable is meant to be run.

---

#### `builder`
^attr-builder

- type: [`path`](path.md) or [`string`](nix/language/data-types/string.md)
- required: yes

Path to an executable that will perform the build.

See [args](#^attr-args) to specify args passed to the builder executable and [builder-execution](#^builder-execution) to understand the environment in which the builder is executed.

> [!example]
> 
> Use the file located at `/bin/bash` as the builder executable:
> 
> ```nix
> derivation {
>   # ...
>   builder = "/bin/bash";
>   # ...
> };
> ```

> [!example]
> 
> Use a file from another derivation as the builder executable:
> 
> ```nix
> derivation {
>   # ...
>   builder = "${pkgs.python}/bin/python";
>   # ...
> };
> ```

---

#### `args`
^attr-args

- type: [`list`](list.md) of [`string`](nix/language/data-types/string.md)
- required: no
- default: `[]`

Command-line arguments to be passed to the [`builder`](#^param-builder) executable.

> [!example]
> 
> Pass arguments to Bash to interpret a shell command:
> 
> ```nix
> derivation {
>   # ...
>   builder = "/bin/bash";
>   args = [ "-c" "echo hello world > $out" ];
>   # ...
> };
> ```

---

#### `outputs`
^attr-outputs

- type: [`list`](list.md) of [`string`](nix/language/data-types/string.md)
- required* no
- default: `[ "out" ]`

---

## Builder Execution
^builder-execution

See [builder execution](https://nixos.org/manual/nix/stable/language/derivations#builder-execution) in official docs.

## References

- https://nixos.org/manual/nix/stable/language/derivations