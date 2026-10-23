### ==Methods==
|Syntax|Category|Description|Example|
|---|---|---|---|
|`col.alias(*alias, **kwargs)`|Naming|Assigns a name (or names for multi-column output) to the column|`col('age').alias('years')`|
|`col.name(*alias, **kwargs)`|Naming|Alias for `alias()`|`col('age').name('years')`|
|`col.cast(dataType)`|Type|Casts the column to a different data type|`col('age').cast('string')`|
|`col.astype(dataType)`|Type|Alias for `cast()`|`col('age').astype(IntegerType())`|
|`col.asc()`|Sorting|Returns column sorted ascending (nulls first)|`df.orderBy(col('age').asc())`|
|`col.asc_nulls_first()`|Sorting|Ascending with nulls sorted first explicitly|`df.orderBy(col('age').asc_nulls_first())`|
|`col.asc_nulls_last()`|Sorting|Ascending with nulls sorted last|`df.orderBy(col('age').asc_nulls_last())`|
|`col.desc()`|Sorting|Returns column sorted descending (nulls last)|`df.orderBy(col('age').desc())`|
|`col.desc_nulls_first()`|Sorting|Descending with nulls sorted first|`df.orderBy(col('age').desc_nulls_first())`|
|`col.desc_nulls_last()`|Sorting|Descending with nulls sorted last explicitly|`df.orderBy(col('age').desc_nulls_last())`|
|`col.isNull()`|Null Check|Returns `True` where value is null|`df.filter(col('age').isNull())`|
|`col.isNotNull()`|Null Check|Returns `True` where value is not null|`df.filter(col('age').isNotNull())`|
|`col.isNaN()`|Null Check|Returns `True` where value is `NaN` (float only)|`df.filter(col('score').isNaN())`|
|`col.isin(*cols)`|Filtering|Returns `True` if value is in the given list|`col('dept').isin('HR','IT','Finance')`|
|`col.like(pattern)`|Filtering|SQL `LIKE` pattern match (case-sensitive)|`col('name').like('A%')`|
|`col.rlike(pattern)`|Filtering|Regex match|`col('name').rlike('^A.*')`|
|`col.startswith(str)`|Filtering|Returns `True` if string starts with given prefix|`col('name').startswith('Ali')`|
|`col.endswith(str)`|Filtering|Returns `True` if string ends with given suffix|`col('name').endswith('ice')`|
|`col.contains(str)`|Filtering|Returns `True` if string contains the substring|`col('name').contains('li')`|
|`col.between(lower, upper)`|Filtering|Returns `True` if value is between lower and upper (inclusive)|`col('age').between(18, 60)`|
|`col.when(condition, value)`|Conditional|Equivalent to `F.when()` chained on a column|`col('age').when(col('age')>18, 'adult')`|
|`col.otherwise(value)`|Conditional|Default value when no `when()` condition matches|`F.when(...).otherwise('unknown')`|
|`col.over(window)`|Window|Applies the column expression over a window spec|`F.rank().over(Window.orderBy('sal'))`|
|`col.substr(startPos, length)`|String|Returns substring from startPos of given length|`col('name').substr(1, 3)`|
|`col.getItem(key)`|Complex|Gets item at index from array, or by key from map|`col('items').getItem(0)`|
|`col.getField(name)`|Complex|Gets a field from a struct column|`col('address').getField('city')`|
|`col.dropFields(*fieldNames)`|Complex|Drops fields from a struct column|`col('address').dropFields('zip')`|
|`col.withField(fieldName, col)`|Complex|Adds or replaces a field in a struct column|`col('addr').withField('pin', lit('600001'))`|
|`col.eqNullSafe(other)`|Comparison|Null-safe equality check (`<=>` in SQL)|`col('a').eqNullSafe(col('b'))`|
|`col.bitwiseAND(other)`|Bitwise|Bitwise AND|`col('flags').bitwiseAND(3)`|
|`col.bitwiseOR(other)`|Bitwise|Bitwise OR|`col('flags').bitwiseOR(1)`|
|`col.bitwiseXOR(other)`|Bitwise|Bitwise XOR|`col('flags').bitwiseXOR(2)`|
---
### Operators (Python dunder / infix)
|Operator|Description|Example|
|---|---|---|
|`+`|Addition (numeric) or string concat|`col('a') + col('b')`|
|`-`|Subtraction|`col('price') - col('discount')`|
|`*`|Multiplication|`col('qty') * col('price')`|
|`/`|Division|`col('total') / col('count')`|
|`%`|Modulo|`col('id') % 2`|
|`**`|Power|`col('x') ** 2`|
|`==`|Equality (`__eq__`)|`col('status') == 'active'`|
|`!=`|Inequality (`__ne__`)|`col('status') != 'inactive'`|
|`>`|Greater than|`col('age') > 18`|
|`<`|Less than|`col('age') < 60`|
|`>=`|Greater than or equal|`col('score') >= 90`|
|`<=`|Less than or equal|`col('score') <= 100`|
|`&`|Logical AND (use instead of `and`)|`(col('a')>1) & (col('b')<5)`|
|`\|`|Logical OR (use instead of `or`)|`(col('a')==1) \| (col('b')==2)`|
|`~`|Logical NOT (use instead of `not`)|`~col('active')`|
|`[]` (`__getitem__`)|Access element of array/map/struct by index or key|`col('items')[0]`, `col('map')['key']`|
---
**Two common gotchas:**
- Always wrap boolean conditions in `()` when using `&` / `|` — operator precedence in Python will break your logic otherwise: `(col('a')>1) & (col('b')<5)` not `col('a')>1 & col('b')<5`
- Use `==` not `is` for column equality, and `&`/`|`/`~` not `and`/`or`/`not` — the Python keywords don't work on Column objects