### Creation
```python
a = [1, 2, 3]
b = list(range(5))      # [0,1,2,3,4]
c = [0] * 3             # [0,0,0]
d = [x**2 for x in range(5)]  # list comprehension → [0,1,4,9,16]
```
---
### Indexing & Slicing
```python
a = [10, 20, 30, 40, 50]
a[0]       # 10  (first)
a[-1]      # 50  (last)
a[1:3]     # [20, 30]
a[::-1]    # [50, 40, 30, 20, 10]  (reversed)
a[::2]     # [10, 30, 50]  (every 2nd)
```
---
### All Methods
|Method|What it does|Example|Result|
|---|---|---|---|
|`.append(x)`|Add to end|`a.append(4)`|`[1,2,3,4]`|
|`.insert(i, x)`|Insert at index|`a.insert(1, 9)`|`[1,9,2,3]`|
|`.extend(iter)`|Add all from iterable|`a.extend([4,5])`|`[1,2,3,4,5]`|
|`.remove(x)`|Remove first occurrence|`a.remove(2)`|`[1,3]`|
|`.pop(i)`|Remove & return at index|`a.pop(0)`|returns `1`|
|`.pop()`|Remove & return last|`a.pop()`|returns `3`|
|`.clear()`|Remove all elements|`a.clear()`|`[]`|
|`.index(x)`|First index of value|`a.index(2)`|`1`|
|`.count(x)`|Count occurrences|`a.count(2)`|`1`|
|`.sort()`|Sort in-place|`a.sort()`|`[1,2,3]`|
|`.sort(reverse=True)`|Sort descending|`a.sort(reverse=True)`|`[3,2,1]`|
|`.reverse()`|Reverse in-place|`a.reverse()`|`[3,2,1]`|
|`.copy()`|Shallow copy|`b = a.copy()`|new list|
---
### Built-in Functions on Lists
|Function|Purpose|Example|Result|
|---|---|---|---|
|`len(a)`|Length|`len([1,2,3])`|`3`|
|`sum(a)`|Sum|`sum([1,2,3])`|`6`|
|`min(a)`|Minimum|`min([1,2,3])`|`1`|
|`max(a)`|Maximum|`max([1,2,3])`|`3`|
|`sorted(a)`|Returns new sorted list|`sorted([3,1,2])`|`[1,2,3]`|
|`reversed(a)`|Returns iterator|`list(reversed(a))`|`[3,2,1]`|
|`enumerate(a)`|Index + value pairs|`list(enumerate(a))`|`[(0,1),(1,2)...]`|
|`zip(a, b)`|Pair two lists|`list(zip([1,2],[3,4]))`|`[(1,3),(2,4)]`|
|`any(a)`|Any truthy?|`any([0,1,0])`|`True`|
|`all(a)`|All truthy?|`all([1,1,1])`|`True`|
|`map(f, a)`|Apply function|`list(map(str, [1,2]))`|`['1','2']`|
|`filter(f, a)`|Filter elements|`list(filter(None,[0,1,2]))`|`[1,2]`|
---
### List Comprehensions
```python
# Basic
squares = [x**2 for x in range(5)]
# With condition
evens = [x for x in range(10) if x % 2 == 0]
# Nested
matrix = [[i*j for j in range(3)] for i in range(3)]
# Flatten nested list
flat = [x for row in matrix for x in row]
```
---
### Copying — Shallow vs Deep
```python
import copy
a = [[1, 2], [3, 4]]
b = a.copy()          # shallow — inner lists still shared
c = copy.deepcopy(a)  # deep — fully independent copy
a[0][0] = 99
print(b[0][0])   # 99  ← shallow copy affected!
print(c[0][0])   # 1   ← deep copy safe ✅
```
---
### Performance
|Operation|Time Complexity|
|---|---|
|`a[i]` index access|O(1)|
|`a.append(x)`|O(1) amortized|
|`a.pop()` (last)|O(1)|
|`a.pop(0)` (first)|O(n) ⚠️|
|`a.insert(0, x)`|O(n) ⚠️|
|`x in a`|O(n)|
|`a.sort()`|O(n log n)|
|`a.reverse()`|O(n)|
- For frequent insert/delete at front → use `collections.deque` instead
- For large numeric data → use `numpy` arrays (very common in India's DS/ML community)
---
### Best Practices
- Prefer **list comprehension** over `map()`/`filter()` — more Pythonic
- Use `a[:]` or `a.copy()` to avoid mutating original
- Use `collections.deque` for queue-like operations
- Avoid `a.pop(0)` in loops — it's O(n); use `deque.popleft()` instead
- Use `numpy` lists for heavy numerical work (data science, ML — hugely popular in India's startup/IT scene)