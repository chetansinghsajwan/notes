# CSS

While HTML is used to define the structure and semantics of your content, CSS is used to style it and lay it out.

## What is CSS for?

CSS is a language for specifying how documents are presented to users — how they are styled, laid out, etc.

A **document** is usually a text file structured using a markup language — [HTML](https://developer.mozilla.org/en-US/docs/Glossary/HTML) is the most common markup language, but you may also come across other markup languages such as [SVG](https://developer.mozilla.org/en-US/docs/Glossary/SVG) or [XML](https://developer.mozilla.org/en-US/docs/Glossary/XML).

> [!note]
> A browser is sometimes called a [user agent](https://developer.mozilla.org/en-US/docs/Glossary/User_agent), which basically means a computer program that represents a person inside a computer system. Browsers are the main type of user agents we think of when talking about CSS, however, they are not the only ones. There are other user agents available, such as those that convert HTML and CSS documents into PDFs to be printed.

## [CSS specifications](https://developer.mozilla.org/en-US/docs/Learn/CSS/First_steps/What_is_CSS#css_specifications)

All web standards technologies (HTML, CSS, JavaScript, etc.) are defined in giant documents called specifications (or "specs"), which are published by standards organizations (such as the [W3C](https://developer.mozilla.org/en-US/docs/Glossary/W3C), [WHATWG](https://developer.mozilla.org/en-US/docs/Glossary/WHATWG), [ECMA](https://developer.mozilla.org/en-US/docs/Glossary/ECMA), or [Khronos](https://developer.mozilla.org/en-US/docs/Glossary/Khronos)) and define precisely how those technologies are supposed to behave.

CSS is no different — it is developed by a group within the W3C called the [CSS Working Group](https://www.w3.org/Style/CSS/). This group is made of representatives of browser vendors and other companies who have an interest in CSS. There are also other people, known as _invited experts_, who act as independent voices; they are not linked to a member organization.

New CSS features are developed or specified by the CSS Working Group — sometimes because a particular browser is interested in having some capability, other times because web designers and developers are asking for a feature, and sometimes because the Working Group itself has identified a requirement. CSS is constantly developing, with new features becoming available. However, a key thing about CSS is that everyone works very hard to never change things in a way that would break old websites. A website built in 2000, using the limited CSS available then, should still be usable in a browser today!

## [Changing the default behavior of elements](https://developer.mozilla.org/en-US/docs/Learn/CSS/First_steps/Getting_started#changing_the_default_behavior_of_elements)

When we look at a well-marked up HTML document, even something as simple as our example, we can see how the browser is making the HTML readable by adding some default styling. Headings are large and bold and our list has bullets. This happens because browsers have internal stylesheets containing default styles, which they apply to all pages by default; without them all of the text would run together in a clump and we would have to style everything from scratch. All modern browsers display HTML content by default in pretty much the same way.

## References

- https://developer.mozilla.org/en-US/docs/Learn/CSS
