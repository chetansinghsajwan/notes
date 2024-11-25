# `Boolean` Object

## Instance Properties

###### `constructor`

It is a reference to the constructor function that created the instance object.

**Writable:** yes
**Enumerable:** no
**Configurable:** yes



## Instance Methods

###### `toString()`

**Returns:**

A string representing the boolean value, which is either `"true"` or `"false"`.

**Overrides:** [`Object.toString()`](git/concepts/object.md#^to_string)

**Example:**

```js
const flag = new Boolean(true);
console.log(flag.toString()); // "true"
console.log(false.toString()); // "false"
```

###### `valueOf()`

**Returns:**

Primitive value of the given object.

**Description:**

This method is usually called internally by JavaScript and not explicitly in code.

**Overrides:** [`Object.valueOf()`](git/concepts/object.md#^value_of)

## References

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean