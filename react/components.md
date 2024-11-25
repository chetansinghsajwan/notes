# Components

## Why do component names need to start with capital letters?

In `JSX`, React components are written in a syntax that gets transformed into plain JavaScript using the `React.createElement API`, thanks to `Babel`. Here’s where the capital letter comes in:

When `Babel` encounters a name starting with a `capital letter`, it knows it’s dealing with a `React component` and converts it into a `React Fiber object` (a key part of React’s rendering system).

## References

 - https://dev.to/bhataasim/why-do-react-components-need-to-start-with-capital-letters-4c8k
 