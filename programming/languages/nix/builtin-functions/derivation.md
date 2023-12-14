# `derivation` function

## Parameters

---

#### `name`
^param-name

**type:** [`string`](programming/languages/nix/data-types/string)
**required:** yes

Name of the derivation.

It is embedded into the store path of the corresponding store derivation 

> [!example]
> 
> For `name = "hello"`, the store derivation's path will be `/nix/store/<hash>-hello.drv` and the [output](#^param-outputs) paths will be of the form `/nix/store/<hash>-hello[-<output>]`

---

#### `system`
^param-system

**type:** [`string`](programming/languages/nix/data-types/string)
**required:** yes

The system type on which the [`builder`](#^param-builder) executable is meant to be run.

---

#### `builder`
^param-builder

**type:** [`path`](programming/languages/nix/data-types/path) or [`string`](programming/languages/nix/data-types/string)
**required:** yes

Path to an executable that will perform the build.

> [!related]
> 
> [args](#^param-args)

---

#### `args`
^param-args

**type:** [`list`](programming/languages/nix/data-types/list) of [`string`](programming/languages/nix/data-types/string)
**default:** `[]`
**required:** no

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
^param-outputs

**type:** [`list`](programming/languages/nix/data-types/list) of [`string`](programming/languages/nix/data-types/string)
**required**: no
**default**: `[ "out" ]`

---

## References

> https://nixos.org/manual/nix/stable/language/derivations