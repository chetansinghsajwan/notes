`Row` is a very lean class — it has only **one explicit method** (`asDict`) and everything else is via Python dunder/operator support since `Row` extends `tuple`.
|Syntax|Type|Description|Example|
|---|---|---|---|
|`Row(**kwargs)`|Constructor|Create a named Row (like a struct)|`Row(name='Alice', age=11)`|
|`Row(*args)`|Constructor|Create an unnamed Row (positional, tuple-style)|`Row('Alice', 11)`|
|`Row('field1', 'field2')`|Factory|Create a Row schema/class template to reuse|`Person = Row('name','age'); Person('Alice',11)`|
|`row['field']`|`__getitem__`|Access field by name (string key)|`row['name']`|
|`row[0]`|`__getitem__`|Access field by index (integer)|`row[0]`|
|`row.field`|`__getattr__`|Access named field as attribute|`row.name`|
|`row.asDict(recursive=False)`|method|Convert Row to a Python dict. Set `recursive=True` to also convert nested Rows|`row.asDict()` → `{'name':'Alice','age':11}`|
|`'field' in row`|`__contains__`|Check if a field name exists in the Row|`'name' in row` → `True`|
|`len(row)`|`__len__`|Number of fields in the Row|`len(row)` → `2`|
|`list(row)`|`__iter__`|Iterate over field values|`list(row)` → `['Alice', 11]`|
|`row1 == row2`|`__eq__`|Equality check between two Rows|`Row(a=1) == Row(a=1)` → `True`|
|`row < row2` / `row > row2`|`__lt__` / `__gt__`|Comparison (inherited from tuple)|`Row(1) < Row(2)` → `True`|
|`str(row)` / `repr(row)`|`__repr__`|Human-readable string representation|`str(row)` → `"Row(name='Alice', age=11)"`|
|`row[1:3]`|`__getitem__` (slice)|Slice fields like a tuple|`row[0:1]` → `('Alice',)`|
**A few things worth noting:**
- `Row` extends Python's built-in `tuple`, so all tuple operations work on it
- Named fields are only available when the Row was created with keyword args — positional Rows don't support `row.field` or `'field' in row`
- `asDict(recursive=True)` is very useful when you have nested Rows (e.g. struct columns) and want a plain Python dict
- In `foreach` / `foreachPartition`, every element you receive is a `Row` — these operators are how you extract values from it