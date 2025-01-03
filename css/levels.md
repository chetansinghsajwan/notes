# CSS Levels

Cascading Style Sheets does not have versions in the traditional sense; instead it has levels[](https://www.w3.org/TR/css-2023/#levels). Each level of CSS builds on the previous, refining definitions and adding features. The feature set of each higher level is a superset of any lower level, and the behavior allowed for a given feature in a higher level is a subset of that allowed in the lower levels. A user agent conforming to a higher level of CSS is thus also conformant to all lower levels.

## CSS Level 1

The CSS Working Group considers the [CSS1 specification](https://www.w3.org/TR/2008/REC-CSS1-20080411/) to be obsolete. [CSS Level 1](https://www.w3.org/TR/css-2023/#css-level-1) is defined as all the features defined in the CSS1 specification (properties, values, at-rules, etc), but using the syntax and definitions in the [CSS2.1 specification](https://www.w3.org/TR/CSS2/). [CSS Style Attributes](https://www.w3.org/TR/css-style-attr/) defines its inclusion in element-specific style attributes.

CSS Level 2

Although the [CSS2 specification](https://www.w3.org/TR/2008/REC-CSS2-20080411/) is technically a W3C Recommendation, it passed into the Recommendation stage before the W3C had defined the Candidate Recommendation stage. Over time implementation experience and further review has brought to light many problems in the CSS2 specification, so instead of expanding an already [unwieldy errata list](https://www.w3.org/Style/css2-updates/REC-CSS2-19980512-errata.html), the CSS Working Group chose to define CSS Level 2 Revision 1 (CSS2.1). In case of any conflict between the two specs CSS2.1 contains the definitive definition.

Once CSS2.1 became Candidate Recommendation—effectively though not officially the same level of stability as CSS2—obsoleted the CSS2 Recommendation. Features in CSS2 that were dropped from CSS2.1 should be considered to be at the Candidate Recommendation stage, but note that many of these have been or will be pulled into a CSS Level 3 working draft, in which case that specification will, once it reaches CR, obsolete the definitions in CSS2.

The [CSS2.1 specification](https://www.w3.org/TR/CSS2/) defines [CSS Level 2](https://www.w3.org/TR/css-2023/#css-level-2) and the [CSS Style Attributes specification](https://www.w3.org/TR/css-style-attr/) defines its inclusion in element-specific style attributes.

CSS Level 3

[CSS Level 3](https://www.w3.org/TR/css-2023/#css-level-3) builds on CSS Level 2 module by module, using the CSS2.1 specification as its core. Each module adds functionality and/or replaces part of the CSS2.1 specification. The CSS Working Group intends that the new CSS modules will not contradict the CSS2.1 specification: only that they will add functionality and refine definitions. As each module is completed, it will be plugged in to the existing system of CSS2.1 plus previously-completed modules.

From this level on modules are levelled independently: for example Selectors Level 4 may well be completed before CSS Line Module Level 3. Modules with no [CSS Level 2](https://www.w3.org/TR/css-2023/#css-level-2) equivalent start at Level 1; modules that update features that existed in CSS Level 2 start at Level 3.

CSS Level 4[](https://www.w3.org/TR/css-2023/#css-level-4) and beyond

There is no CSS Level 4. Independent modules can reach level 4 or beyond, but CSS the language no longer has levels. ("CSS Level 3" as a term is used only to differentiate it from the previous monolithic versions.)

## References

- https://www.w3.org/TR/css-2023/#css-levels
