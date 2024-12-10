# Selectors

It is a pattern of elements and other terms that tell the browser which HTML elements should be selected to have the CSS property values inside the rule applied to them. The element or elements which are selected by the selector are referred to as the _subject of the selector_.

## Combining selectors

If you have more than one thing which uses the same CSS then the individual selectors can be combined into a _selector list_ so that the rule is applied to all of the individual selectors. For example, if I have the same CSS for an `h1` and also a class of `.special`, I could write this as two separate rules.

```css
h1 {
  color: blue;
}

.special {
  color: blue;
}
```

I could also combine these into a selector list, by adding a comma between them.

```css
h1, .special {
  color: blue;
}
```

White space is valid before or after the comma. You may also find the selectors more readable if each is on a new line.

```css
h1,
.special {
  color: blue;
}
```

In the live example below try combining the two selectors which have identical declarations. The visual display should be the same after combining them.

When you group selectors in this way, if any selector is syntactically invalid, the whole rule will be ignored.

In the following example, the invalid class selector rule will be ignored, whereas the `h1` would still be styled.

```css
h1 {
  color: blue;
}

..special {
  color: blue;
}
```

When combined however, neither the `h1` nor the class will be styled as the entire rule is deemed invalid.

```css
h1, ..special {
  color: blue;
}
```

## References

- https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors
