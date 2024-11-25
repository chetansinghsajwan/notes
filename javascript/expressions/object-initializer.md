---
status: completed
---

# Object Initializer Expression

It is a comma-delimited list of zero or more pairs of property names and associated values of an object, enclosed in curly braces `{}`.

---
**Example**

```js
const object = { a: 'foo', b: 42, c: {} }

// prints: 'foo'
console.log(object1.a)
```

---
## Syntax

```js
o = {
  a: "foo",
  b: 42,
  c: {},
  1: "number literal property",
  "foo:bar": "string literal property",

  shorthandProperty,

  method(parameters) {
    // …
  },

  get property() {},
  set property(value) {},

  [expression]: "computed property",

  __proto__: prototype,

  ...spreadProperty,
};
```

## Description

An object initializer is an expression that describes the initialization of an [`Object`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object). Objects consist of _properties_, which are used to describe an object. The values of object properties can either contain [primitive](https://developer.mozilla.org/en-US/docs/Glossary/Primitive) data types or other objects.

### Shorthand Notation

```js
const a = "foo";
const b = 42;
const c = {};

const obj1 = {
  a: a,
  b: b,
  c: c,
};

// you can define obj1 as this, they create the same object
const obj2 = {
  a,
  b,
  c,
};
```

### Duplicate property names

When using the same name for your properties, the second property will overwrite the first.

```js
const a = { x: 1, x: 2 }

// prints {x: 2}
console.log(a)
```

### Method definitions

A property of an object can also refer to a [function](javascript/function) or a [getter](javascript/function/getter) or [setter](javascript/function/getter) method.

```js
const o = {
  property: function (parameters) {},
  get property() {},
  set property(value) {},
};
```

A shorthand notation is available, so that the keyword `function` is no longer necessary.

```js
// Shorthand method names
const o = {
  property(parameters) {},
};
```

### Computed Properties

The object initializer syntax also supports computed property names. That allows you to put an expression in square brackets `[]`, that will be computed and used as the property name.

> [!note]
> This is not same as the bracket notation of the [property accessor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_accessors) syntax

```js
// Computed property names
let i = 0;
const a = {
  [`foo${++i}`]: i,
  [`foo${++i}`]: i,
  [`foo${++i}`]: i,
};

console.log(a.foo1); // 1
console.log(a.foo2); // 2
console.log(a.foo3); // 3

const items = ["A", "B", "C"];
const obj = {
  [items]: "Hello",
};
console.log(obj); // A,B,C: "Hello"
console.log(obj["A,B,C"]); // "Hello"

const param = "size";
const config = {
  [param]: 12,
  [`mobile${param.charAt(0).toUpperCase()}${param.slice(1)}`]: 4,
};

console.log(config); // {size: 12, mobileSize: 4}
```

### Spread Properties

Object literals support the [spread syntax](javascript/expressions/spread-syntax). It copies own enumerable properties from a provided object onto a new object.

See [spread syntax for object literals](javascript/expressions/spread-syntax#object-literal) for more details.

```js
const obj1 = { foo: "bar", x: 42 };
const obj2 = { foo: "baz", y: 13 };

const clonedObj = { ...obj1 };
// { foo: "bar", x: 42 }

const mergedObj = { ...obj1, ...obj2 };
// { foo: "baz", x: 42, y: 13 }
```

### Prototype Setter

A property definition of the form `__proto__: value` or `"__proto__": value` does not create a property with the name `__proto__`. Instead, if the provided value is an object or [`null`](javascript/operators/null), it points the `[[Prototype]]` of the created object to that value. If the value is not an object or `null`, the object is not changed.

Only a single prototype setter is permitted in an object literal. Multiple prototype setters are a syntax error.

```js
const obj1 = {};
console.log(Object.getPrototypeOf(obj1) === Object.prototype); // true

const obj2 = { __proto__: null };
console.log(Object.getPrototypeOf(obj2)); // null

const protoObj = {};
const obj3 = { "__proto__": protoObj };
console.log(Object.getPrototypeOf(obj3) === protoObj); // true

const obj4 = { __proto__: "not an object or null" };
console.log(Object.getPrototypeOf(obj4) === Object.prototype); // true
console.log(Object.hasOwn(obj4, "__proto__")); // false
```

## References

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer
