# CSS Color Type

A `<color>` value can be specified using one of the methods listed below:

- By keywords: [`<named-color>`](https://developer.mozilla.org/en-US/docs/Web/CSS/named-color) (such as `blue` or `pink`), [`<system-color>`](https://developer.mozilla.org/en-US/docs/Web/CSS/system-color), and [`currentcolor`](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value#currentcolor_keyword).
- By hexadecimal notations: [`<hex-color>`](https://developer.mozilla.org/en-US/docs/Web/CSS/hex-color) (such as `#ff0000`).
- By `<color-function>`, with parameters in a [color space](https://developer.mozilla.org/en-US/docs/Glossary/Color_space) using functional notations:
    - [sRGB](https://en.wikipedia.org/wiki/SRGB) color space: [`hsl()`](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/hsl), [`hwb()`](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/hwb), and [`rgb()`](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/rgb).
    - [CIELAB](https://en.wikipedia.org/wiki/CIELAB_color_space) color space: [`lab()`](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/lab) and [`lch()`](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/lch).
    - [Oklab](https://bottosson.github.io/posts/oklab/) color space: [`oklab()`](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/oklab) and [`oklch()`](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/oklch).
    - Other color spaces: [`color()`](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/color).
- By using [relative color](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_colors/Relative_colors) syntax to output a new color based on an existing color. Any of the above color functions can take an **origin color** preceded by the `from` keyword and followed by definitions of the channel values for the new **output color**.
- By mixing two colors: [`color-mix()`](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/color-mix).
- By specifying two colors, using the first for light color-schemes and the second for dark color-schemes: [`light-dark()`](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/light-dark).