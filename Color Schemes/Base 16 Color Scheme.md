Base16 is a standardized color scheme system for coding and terminals.
**Core idea**
- A palette of exactly **16 colors** (base00 to base0F)
- Created by Chris Kempson; now community-maintained under the "tinted-theming" project
- One theme definition works across hundreds of apps (editors, terminals, browsers, etc.)
**How the 16 slots are used**
|Slots|Purpose|
|---|---|
|base00 – base07|Grayscale ramp: darkest (background) to lightest (foreground)|
|base08 – base0F|Accent colors: red, orange, yellow, green, cyan, blue, purple, brown|
**Key slot meanings**
- **base00**: default background
- **base01**: lighter background (status bars, line numbers)
- **base02**: selection background
- **base03**: comments, invisible characters
- **base05**: default foreground text
- **base08**: variables, errors, deletions
- **base0B**: strings, additions
- **base0D**: functions, headings
**Why it's popular**
- Write a theme once, apply it everywhere via templates
- Consistent look across tools
- Hundreds of ready-made schemes (Solarized, Gruvbox, Nord and Dracula-style variants, etc.)
- Free and open source, so no cost (₹0)
**Limitation**
- Only 16 colors, so syntax highlighting is less nuanced than in themes with more colors
**Where to find it**
- GitHub: `tinted-theming/home`, which includes the spec, schemes and templates
- Note: the project has also evolved into **Base24**, which adds 8 more colors
---
==What other schemes are there?==
|System|Type|Key idea|
|---|---|---|
|ANSI 16 colors|Terminal standard|The original 8 + 8 bright colors; Base16 maps onto this|
|Base24|Base16 extension|24 slots (adds 8 extra shades); same tinted-theming project|
|Base46|Base16-inspired|NvChad's system for Neovim, with more UI/syntax slots|
|Material Design / Material You|Design system|Generates a full palette from one seed color (HCT color space)|
|Open Color|Design palette|13 hues × 10 shades, tuned for UI|
|Pywal|Generator|Builds a 16-color scheme automatically from your wallpaper|
|Radix Colors|Design system|12-step scales per hue, with light/dark pairs|
|Tailwind palette|Design system|11 shades (50–950) per hue|
|xterm 256 colors|Terminal standard|16 ANSI + 216 color cube + 24 grays|
**Good to know**
- **Closest to Base16:** Base24, Base46 and Pywal (all fixed-slot palettes for code/terminals).
- **Different approach:** Material, Radix and Tailwind are _scales per hue_ for app/web UI, not syntax-slot systems.
- **Related tools:** Stylix (NixOS) and Flavours use Base16 to theme a whole system.
Want a deeper comparison of any two?
==How compatible are ANSI 16 with base 16? Why are they even separate then?==
**Short answer:** partly compatible. Base16 can be mapped onto ANSI 16, but not cleanly.
**Standard mapping (sorted by ANSI index)**
|ANSI index|ANSI name|Base16 slot|
|---|---|---|
|0|Black|base00|
|1|Red|base08|
|2|Green|base0B|
|3|Yellow|base0A|
|4|Blue|base0D|
|5|Magenta|base0E|
|6|Cyan|base0C|
|7|White|base05|
|8|Bright black|base03|
|9-14|Bright red to cyan|Same as 1-6 (duplicates)|
|15|Bright white|base07|
**Where it doesn't fit**
- Base16 has **8 accents**, ANSI has only **6 hues**, so orange (base09) and brown (base0F) have no slot.
- Base16's extra grays (base01, 02, 04, 06) also have no slot.
- Fix: the `base16-shell` scripts redefine **256-color slots 16-21** to hold these leftovers.
- Bright colors just repeat the normal ones, so the palette effectively has fewer distinct colors.
**Why they're separate**
||ANSI 16|Base16|
|---|---|---|
|Nature|Terminal protocol (1970s-80s)|Palette design spec (2010s)|
|Defines|Slot _names_ only|Slot _roles_ and hex values|
|Actual colors|Chosen by the terminal or user|Fixed by each scheme|
|Meaning|"Color #1 is red"|"base08 is variables/errors"|
|Scope|Terminals only|Editors, terminals, browsers, apps|
**Takeaway**
- ANSI answers "how does a program _ask_ for a color?"
- Base16 answers "what should this color _be_ and _mean_?"
- Base16 sits on top of ANSI for terminals, not in place of it.
==Should I use base24 or 46 over 16? Since it gives me more range?==
**Short answer:** not automatically. More slots only help if your tools support them.
**Comparison (sorted by slot count)**
|System|Slots|Ecosystem size|Best for|
|---|---|---|---|
|Base16|16|Largest (hundreds of schemes, templates for nearly every app)|Broad compatibility|
|Base24|24|Smaller, but growing|Terminals needing true bright colors|
|Base46|Base16 + extras|Only NvChad (Neovim)|NvChad users|
**Base24**
- Adds 8 slots: darker backgrounds plus proper bright red-to-cyan variants
- Fixes the ANSI "duplicate bright colors" problem I mentioned
- Fewer apps have templates, so some tools won't be themed
- Base24 schemes are usually backward-compatible: the first 16 slots match Base16 meaning
**Base46**
- Tied to NvChad; not a general standard
- Skip it unless you use NvChad
**Recommendation**
- **Terminal-heavy, want distinct bright colors:** Base24
- **Themeing many different apps:** Base16
- **NvChad user:** Base46 (already built in)
- **Unsure:** start with Base16; switching later is easy since most Base24 schemes contain a Base16 subset
**Caveat**
- More range doesn't mean better looks. Scheme quality (contrast, harmony) matters more than slot count.