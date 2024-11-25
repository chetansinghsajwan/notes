# `import` Statement

When an [`import`](https://docs.python.org/3/reference/simple_stmts.html#import) statement is executed, the standard builtin [`__import__()`](python/functions/__import__) function is called.

See [importing](python/importing) to understand the logic behind importing.

## Relative Paths

Relative imports must start with a single or double dot. A single leading dot indicates a relative import, starting with the current package. Two or more leading dots indicate a relative import to the parent(s) of the current package, one level per dot after the first.

For example, given the following package layout:

```
package/
    __init__.py
    subpackage1/
        __init__.py
        moduleX.py
        moduleY.py
    subpackage2/
        __init__.py
        moduleZ.py
    moduleA.py
```

In either `subpackage1/moduleX.py` or `subpackage1/__init__.py`, the following are valid relative imports:

```python
from .moduleY import spam
from .moduleY import spam as ham
from . import moduleY
from ..subpackage1 import moduleY
from ..subpackage2.moduleZ import eggs
from ..moduleA import foo
```

Absolute imports may use either the `import <>` or `from <> import <>` syntax, but relative imports may only use the second form; the reason for this is that:

`import XXX.YYY.ZZZ`

should expose `XXX.YYY.ZZZ` as a usable expression, but .moduleY is not a valid expression.

## References

- https://docs.python.org/3/reference/import.html
- https://docs.python.org/3/reference/import.html#package-relative-imports
