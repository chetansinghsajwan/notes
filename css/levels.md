# CSS Levels

Cascading Style Sheets does not have versions in the traditional sense; instead it has levels[](https://www.w3.org/TR/css-2023/#levels). Each level of CSS builds on the previous, refining definitions and adding features. The feature set of each higher level is a superset of any lower level, and the behavior allowed for a given feature in a higher level is a subset of that allowed in the lower levels. A user agent conforming to a higher level of CSS is thus also conformant to all lower levels.

## CSS Level 1

The CSS Working Group considers the [CSS1 specification](https://www.w3.org/TR/2008/REC-CSS1-20080411/) to be obsolete. [CSS Level 1](https://www.w3.org/TR/css-2023/#css-level-1) is defined as all the features defined in the CSS1 specification (properties, values, at-rules, etc), but using the syntax and definitions in the [CSS2.1 specification](https://www.w3.org/TR/CSS2/). [CSS Style Attributes](https://www.w3.org/TR/css-style-attr/) defines its inclusion in element-specific style attributes.

CSS Level 2

Although the [CSS2 specification](https://www.w3.org/TR/2008/REC-CSS2-20080411/) is technically a W3C Recommendation, it passed into the Recommendation stage before the W3C had defined the Candidate Recommendation stage. Over time implementation experience and further review has brought to light many problems in the CSS2 specification, so instead of expanding an already [unwieldy errata list](https://www.w3.org/Style/css2-updates/REC-CSS2-19980512-errata.html), the CSS Working Group chose to define CSS Level 2 Revision 1 (CSS2.1). In case of any conflict between the two specs CSS2.1 contains the definitive definition.

Once CSS2.1 became Candidate Recommendation—effectively though not officially the same level of stability as CSS2—obsoleted the CSS2 Recommendation. Features in CSS2 that were dropped from CSS2.1 should be considered to be at the Candidate Recommendation stage, but note that many of these have been or will be pulled into a CSS Level 3 working draft, in which case that specification will, once it reaches CR, obsolete the definitions in CSS2.

The [CSS2.1 specification](https://www.w3.org/TR/CSS2/) defines [CSS Level 2](https://www.w3.org/TR/css-2023/#css-level-2) and the [CSS Style Attributes specification](https://www.w3.org/TR/css-style-attr/) defines its inclusion in element-specific style attributes.

## References

- https://www.w3.org/TR/css-2023/#css-levels
