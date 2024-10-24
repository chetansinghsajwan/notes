# First Class Functions

A programming language is said to have **First-class functions** when functions in that language are treated like any other variable.

For example, in such a language, a function can be passed as an argument to other functions, can be returned by another function and can be assigned as a value to a variable.

Examples of such languages are [javascript](javascript/javascript).

```js
const foo = () => {
  console.log("foobar")
}

// Invoke it using the variable
foo()
```

## References

- https://developer.mozilla.org/en-US/docs/Glossary/First-class_Function
