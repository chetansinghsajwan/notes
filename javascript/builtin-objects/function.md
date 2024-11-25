# `Function` Object

**Inherits**: [Object](git/concepts/object.md)

## Properties

### Instance Properties

###### `name`

**Required**: no

The name of the function.

It can be used to identify the function in debugging tools or error messages. It has no semantic significance to the language itself.

It is read-only and cannot be changed by the assignment operator

**Example:**

```js
function doSomething() {}
doSomething.name; // "doSomething"
```

**Example:**

```js
new Function().name; // "anonymous"
```

###### `displayName`

**Required**: no

The display name of the function.

If present, may be preferred by consoles and profilers over the `name` property to be displayed as the name of a function.

###### `length`

**Required**: no

Specifies the number of arguments expected by the function.

This number excludes the [rest parameter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters) and only includes parameters before the first one with a default value

**Example:**

```js
console.log(Function.length); // 1

console.log((() => {}).length); // 0
console.log(((a) => {}).length); // 1
console.log(((a, b) => {}).length); // 2 etc.

console.log(((...args) => {}).length);
// 0, rest parameter is not counted

console.log(((a, b = 1, c) => {}).length);
// 1, only parameters before the first one with
// a default value are counted
```

###### `prototype`

**Value:** [object]()
**Required:** no
**Writable:** yes

It is used when the function is used as a constructor with the [`new`](cpp/operators/new.md) operator. It will become the new object's prototype.

## Methods

###### `toString()`

**Returns:** A [string]() representing the source code of this function.

**Overrides:** [`object.toString()`](git/concepts/object.md#^methods.to_string)

**Description:**

If the `toString()` method is called on built-in function objects, a function created by [`Function.prototype.bind()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind), or other non-JavaScript functions, then `toString()` returns a _native function string_ which looks like

```js
function someName() { [native code] }
```

**Example:**

```js
function sum(a, b) {
  return a + b;
}

console.log(sum.toString());
// Expected output: "function sum(a, b) {
//                     return a + b;
//                   }"

console.log(Math.abs.toString());
// Expected output: "function abs() { [native code] }"
```

## References

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function
