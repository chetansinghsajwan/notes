# Python Wheel

Wheels make the end-to-end installation of Python packages faster for two reasons:

1. All else being equal, wheels are typically **smaller in size** than source distributions, meaning they can move faster across a network.
2. Installing from wheels directly avoids the intermediate step of **building** packages off of the source distribution.

---

A Python `.whl` file is essentially a ZIP (`.zip`) archive with a specially crafted filename that tells installers what Python versions and platforms the wheel will support.

A wheel is a type of [**built distribution**](https://packaging.python.org/glossary/#term-built-distribution). In this case, _built_ means that the wheel comes in a ready-to-install format and allows you to skip the build stage required with source distributions.

**Note**: It’s worth mentioning that despite the use of the term _built_, a wheel doesn’t contain `.pyc` files, or compiled Python bytecode.

A wheel filename is broken down into parts separated by hyphens:

`{dist}-{version}(-{build})?-{python}-{abi}-{platform}.whl`

Each section in `{brackets}` is a **tag**, or a component of the wheel name that carries some meaning about what the wheel contains and where the wheel will or will not work.
---

## References

- https://realpython.com/python-wheels
