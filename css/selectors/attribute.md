# Attribute Selectors

you can use attribute selectors to target elements with certain attributes.

These selectors enable the selection of an element based on the presence of an attribute alone (for example `href`), or on various different matches against the value of the attribute.

| Selector             | Example                         | Description                                                                                                                            |
| -------------------- | ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `[_attr_]`           | `a[title]`                      | Matches elements with an _attr_ attribute (whose name is the value in square brackets).                                                |
| `[_attr_=_value_]`   | `a[href="https://example.com"]` | Matches elements with an _attr_ attribute whose value is exactly _value_ — the string inside the quotes.                               |
| `[_attr_~=_value_]`  | `p[class~="special"]`           | Matches elements with an _attr_ attribute whose value is exactly _value_, or contains _value_ in its (space separated) list of values. |
| `[_attr_\|=_value_]` | `div[lang\|="zh"]`              | Matches elements with an _attr_ attribute whose value is exactly _value_ or begins with _value_ immediately followed by a hyphen.      |

These selectors allow for more advanced matching of substrings inside the value of your attribute. For example, if you had classes of `box-warning` and `box-error` and wanted to match everything that started with the string "box-", you could use `[class^="box-"]` to select them both (or `[class|="box"]` as described in section above).

|Selector|Example|Description|
|---|---|---|
|`[attr^=value]`|`li[class^="box-"]`|Matches elements with an _attr_ attribute, whose value begins with _value_.|
|`[attr$=value]`|`li[class$="-box"]`|Matches elements with an _attr_ attribute whose value ends with _value_.|
|`[attr*=value]`|`li[class*="box"]`|Matches elements with an _attr_ attribute whose value contains _value_ anywhere within the string.|
## Case Sensitivity

If you want to match attribute values case-insensitively you can use the value `i` before the closing bracket. This flag tells the browser to match [ASCII](https://developer.mozilla.org/en-US/docs/Glossary/ASCII) characters case-insensitively. Without the flag the values will be matched according to the case-sensitivity of the document language — in HTML's case it will be case sensitive.

## References

- https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors/Attribute_selectors
