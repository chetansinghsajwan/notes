# ID Selector

The case-sensitive ID selector begins with a `#` rather than a dot character, but is used in the same way as a class selector. However, an ID can be used only once per page, and elements can only have a single `id` value applied to them.

**Note:** The ID selector has high [`specificity`](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity). This means styles applied based on matching an ID selector will overrule styles applied based on other selector, including class and type selectors. Because an ID can only occur once on a page and because of the high specificity of ID selectors, it is preferable to add a class to an element instead of an ID. If using the ID is the only way to target the element — perhaps because you do not have access to the markup and cannot edit it — consider using the ID within an [attribute selector](https://developer.mozilla.org/en-US/docs/Web/CSS/Attribute_selectors), such as `p[id="header"]`. [Learn specificity](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance).

## References

- https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors/Type_Class_and_ID_Selectors#id_selectors
