# Python Wheel

Wheels make the end-to-end installation of Python packages faster for two reasons:

1. All else being equal, wheels are typically **smaller in size** than source distributions, meaning they can move faster across a network.
2. Installing from wheels directly avoids the intermediate step of **building** packages off of the source distribution.

---

A Python `.whl` file is essentially a ZIP (`.zip`) archive with a specially crafted filename that tells installers what Python versions and platforms the wheel will support.

A wheel is a type of [**built distribution**](https://packaging.python.org/glossary/#term-built-distribution). In this case, _built_ means that the wheel comes in a ready-to-install format and allows you to skip the build stage required with source distributions.

**Note**: It’s worth mentioning that despite the use of the term _built_, a wheel doesn’t contain `.pyc` files, or compiled Python bytecode.

---

A wheel filename is broken down into parts separated by hyphens:

```
{dist}-{version}(-{build})?-{python}-{abi}-{platform}.whl
```

Each section in `{brackets}` is a **tag**, or a component of the wheel name that carries some meaning about what the wheel contains and where the wheel will or will not work.

Here’s an illustrative example using a [`cryptography`](https://github.com/pyca/cryptography) wheel:


```
cryptography-2.9.2-cp35-abi3-macosx_10_9_x86_64.whl
```

`cryptography` distributes multiple wheels. Each wheel is a **platform wheel**, meaning it supports only specific combinations of Python versions, Python ABIs, operating systems, and machine architectures. You can break down the naming convention into parts:

- **`cryptography`** is the package name.
    
- **`2.9.2`** is the package version of `cryptography`. A version is a [PEP 440](https://www.python.org/dev/peps/pep-0440/)-compliant string such as `2.9.2`, `3.4`, or `3.9.0.a3`.
    
- **`cp35`** is the [Python tag](https://www.python.org/dev/peps/pep-0425/#python-tag) and denotes the Python implementation and version that the wheel demands. The `cp` stands for [CPython](https://realpython.com/cpython-source-code-guide/), the reference implementation of Python, while the `35` denotes Python [3.5](https://docs.python.org/3/whatsnew/3.5.html). This wheel wouldn’t be compatible with [Jython](https://www.jython.org/), for instance.
    
- **`abi3`** is the ABI tag. ABI stands for [application binary interface](https://docs.python.org/3/c-api/stable.html). You don’t really need to worry about what it entails, but `abi3` is a separate version for the binary compatibility of the Python C API.
    
- **`macosx_10_9_x86_64`** is the platform tag, which happens to be quite a mouthful. In this case it can be broken down further into sub-parts:
    
    - **`macosx`** is the [macOS](https://en.wikipedia.org/wiki/MacOS) operating system.
    - **`10_9`** is the macOS developer tools SDK version used to compile the Python that in turn built this wheel.
    - **`x86_64`** is a reference to x86-64 instruction set architecture.

---

Now let’s turn to a different example. Here’s what you saw in the above case for chardet:

```
chardet-3.0.4-py2.py3-none-any.whl
```

You can break this down into its tags:

- **`chardet`** is the package name.
- **`3.0.4`** is the package version of chardet.
- **`py2.py3`** is the Python tag, meaning the wheel supports Python 2 and 3 with any Python implementation.
- **`none`** is the ABI tag, meaning the ABI isn’t a factor.
- **`any`** is the platform. This wheel will work on virtually any platform.

---

The `py2.py3-none-any.whl` segment of the wheel name is common. This is a **universal wheel** that will install with Python 2 or 3 on any platform with any [ABI](https://stackoverflow.com/a/2456882/7954504). If the wheel ends in `none-any.whl`, then it’s very likely a pure-Python package that doesn’t care about a specific Python ABI or CPU architecture.

---

Another example is the `jinja2` templating engine. If you navigate to the [downloads page](https://pypi.org/project/Jinja2/3.0.0a1/#files) for the Jinja 3.x alpha release, then you’ll see the following wheel:

Text

`Jinja2-3.0.0a1-py3-none-any.whl`

Notice the lack of `py2` here. This is a pure-Python project that will work on any Python 3.x version, but it’s not a universal wheel because it doesn’t support Python 2. Instead, it’s called a **pure-Python wheel**.

---

## Benifits of Wheel

- Wheels install faster than source distributions.
- Wheels are smaller than source distributions.
- Wheels cut `setup.py` execution out of the equation.
- There’s no need for a compiler to install wheels that contain compiled extension modules.
- `pip` automatically generates `.pyc` files in the wheel that match the right Python interpreter.
- Wheels provide consistency by cutting many of the variables involved in installing a package out of the equation.

---

A wheel file is essentially a `.zip` archive. You can unzip it using `unzip` tool.

---
### Security Considerations With Platform Wheels

One feature of wheels worth considering from a user security standpoint is that wheels are [potentially subject to version rot](https://github.com/asottile/no-manylinux#what-why) because they bundle a binary dependency rather than allowing that dependency to be updated by your system package manager.

For example, if a wheel incorporates the [`libfortran`](https://gcc.gnu.org/fortran/) shared library, then distributions of that wheel will use the `libfortran` version that they were bundled with even if you upgrade your own machine’s version of `libfortran` with a package manager such as `apt`, `yum`, or `brew`.

If you’re developing in an environment with heightened security precautions, this feature of some platform wheels is something to be mindful of.

---

## Building Wheels

The first thing you need to do to build a wheel locally is to install `wheel`. It doesn’t hurt to make sure that `setuptools` is up to date, too:

```bash
$ python -m pip install -U wheel setuptools
```

- I don't know wh

## References

- https://realpython.com/python-wheels
