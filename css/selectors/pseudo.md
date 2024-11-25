## What is a pseudo class?

A pseudo-class is a selector that selects elements that are in a specific state, e.g. they are the first element of their type, or they are being hovered over by the mouse pointer.

Pseudo-classes are keywords that start with a colon. For example, `:hover` is a pseudo-class.

---

**Note**

It is valid to write pseudo-classes and elements without any element selector preceding them. `:first-child` is equivalent to `*:first-child`. 

---

### User-action pseudo classes

Some pseudo-classes only apply when the user interacts with the document in some way. These **user-action** pseudo-classes, sometimes referred to as **dynamic pseudo-classes**, act as if a class had been added to the element when the user interacts with it.

Examples include:

- [`:hover`](https://developer.mozilla.org/en-US/docs/Web/CSS/:hover) — mentioned above; this only applies if the user moves their pointer over an element, typically a link.
- [`:focus`](https://developer.mozilla.org/en-US/docs/Web/CSS/:focus) — only applies if the user focuses the element by clicking or using keyboard controls.



## References

- https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors/Pseudo-classes_and_pseudo-elements
