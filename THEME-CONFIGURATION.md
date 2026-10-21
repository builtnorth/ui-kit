# UI Kit Theme Configuration (WordPress theme.json)

## Overview

UI Kit uses **CSS custom properties** that map to WordPress theme.json settings. This means:

✅ **Configure ONCE in your theme** → applies to ALL plugins using ui-kit  
✅ **Runtime configuration** → no need to recompile plugins  
✅ **Theme always wins** → centralized control

## How It Works

1. Your **theme** defines values in `theme.json` under `settings.custom`
2. WordPress outputs them as CSS custom properties: `settings.custom.form.field.padding-x` becomes `--wp--custom--form--field--padding-x`
3. **All plugins** using ui-kit read the same custom properties
4. Changes in theme.json **apply everywhere** at once

Every value has a fallback, so leave out anything you don't want to change.

## Focus (`focus`)

Every keyboard focus ring in the kit (links, buttons, fields, cards, slider controls,
floating controls) reads these, so rings stay the same when no token is set.

| theme.json key | CSS variable | Default |
|---|---|---|
| `focus.width` | `--wp--custom--focus--width` | `2px` |
| `focus.offset` | `--wp--custom--focus--offset` | `2px` |
| `focus.color` | `--wp--custom--focus--color` | `currentColor` |

Filled buttons, fields and floating controls keep their own ring colours so the ring
contrasts with what's behind it; width and offset still come from here.

## Controls (`control`)

Icon buttons: slider and lightbox arrows, close buttons, the accordion toggle,
play/pause, core's navigation hamburger and close, and the search icon button.

| theme.json key | CSS variable | Default |
|---|---|---|
| `control.size` | `--wp--custom--control--size` | `2.75rem` (small ×0.8, large ×1.25) |
| `control.icon-size` | `--wp--custom--control--icon-size` | `0.875rem`, the visible glyph |
| `control.icon-stroke` | `--wp--custom--control--icon-stroke` | `2px` (line weight of every control glyph) |
| `control.radius` | `--wp--custom--control--radius` | `0` |
| `control.offset` | `--wp--custom--control--offset` | `0.5rem` (floating controls from the edge) |
| `control.border-width` | `--wp--custom--control--border-width` | `border-width.x-small`, else `2px` |
| `control.color` | `--wp--custom--control--color` | `currentColor` |
| `control.background` | `--wp--custom--control--background` | `transparent` |
| `control.background-hover` | `--wp--custom--control--background-hover` | `color.tint`, else 10% of the text color |
| `control.border` | `--wp--custom--control--border` | `currentColor` on slider and lightbox arrows (a line); `transparent` on the accordion toggle |

Controls over images and video use the overlay tokens: `control.overlay-background`,
`overlay-background-hover`, `overlay-color`, `overlay-ring` and `overlay-blur`
(default `blur(4px)`).

Core's hamburger and close take the size, radius and glyph size; the search icon
button takes the size and keeps the theme's button radius. Their colors stay with
core and the theme.

## Buttons (`button`)

Buttons are core's Button block (`core/button`). With Kitaris's button variants on
(`blocks.core_extensions.button_variants`, on with the `house-defaults` preset) they have two settings:

- **Style** (core's block styles): Fill (default), Outline (`is-style-*`).
- **Variant** (Kitaris attribute): Primary (default), Secondary, Tertiary, Text Link
  (`is-variant-*` on `.wp-block-button`).

Colours come from one set of tokens per variant. Every style reads them, at rest, on
hover, on keyboard focus, and when a clickable card around the button is hovered.

| theme.json key | CSS variable | Default without the token |
|---|---|---|
| `button.{variant}.background` | `--wp--custom--button--{variant}--background` | Primary, Secondary: the surface's text color. Tertiary: 10% of the text color |
| `button.{variant}.text` | `--wp--custom--button--{variant}--text` | Primary, Secondary: the surface color. Tertiary: the text color |
| `button.{variant}.hover-background` | `--wp--custom--button--{variant}--hover-background` | Optional override. Default: the fill moved 15% toward its own extreme (black if dark, white if light), so hover contrast is never lower than at rest |
| `button.{variant}.hover-text` | `--wp--custom--button--{variant}--hover-text` | Optional override. Default: the label moved 30% toward its own extreme |

`{variant}` is `primary`, `secondary` or `tertiary`. Text Link has only
`button.text-link.text` and `button.text-link.hover-text` (both default to the surrounding
text color).

- **Primary Fill is the theme's button element style** (`styles.elements.button`), so it
  matches core Buttons. Point `elements.button` at the Primary tokens (rest colors only);
  its hover is Kitaris's, the same as every variant's. Without tokens, a Primary Fill button looks like the theme's own buttons.
- **Outline**, as core's: the surrounding text color as text and border, no fill, so it
  contrasts with any background. The variant doesn't change it. On hover (and keyboard focus)
  it takes a light tint of the text color.
- **Text Link** is a link: the surrounding text color (or its tokens), underlined, no fill,
  border or side padding. Like every variant it applies to Fill; an Outline button is an
  outline whatever its variant.
- **On a background of a variant's own color** the variant inverts automatically: Kitaris
  redefines its tokens there from the palette pairs. Buttons inherit the tokens, so nothing
  has to outrank the button styles.
- **Padding and border are core's.** Fill: `calc(0.667em + 2px) calc(1.333em + 2px)`, no
  border. Outline: a 2px border and `0.667em 1.333em`. Both come out the same size, so don't
  set padding or border width on `elements.button` or the Outline variation; the theme sets
  only colors, radius (`elements.button.border.radius`) and font. Form buttons (ui-kit
  `_buttons.scss`) use the same numbers.

**Sizes set only the font size.** Core's padding is in **em**, so it scales with each size's
font size and the sizes always keep the same proportions:

| theme.json key | Default |
|---|---|
| `button.font-size-small` | `0.875em` |
| `button.font-size-large` | `1.25em` |
| `button.icon-size-default` (also `-small`, `-large`) | `1.2em` |
| `button.icon-gap`, `button.icon-gap-large` | `0.67rem`, `1rem` |

The default size takes its font size from the theme's `elements.button`.

## Card surfaces

Image, icon and price cards, Taxonomy Feed grid tiles, and the Card style on features,
stats, slides, timeline steps, logos and groups take the **raised** colour of the
background they sit on: a white card on a dark section, a light gray one on white. Nothing
is set per section. Each background colour names its raised colour in the theme's pairs
map, next to its text pair:

```scss
@use "@siratik/ui-kit/base/colors";

@include colors.kitaris-text-contrast-rules((
	"base": (text: "contrast", raised: "neutral-1"),
	"contrast": (text: "base", raised: "base"),
	"primary": "primary-contrast", // no raised: derived
), $page: "base");
```

- Each `.has-{slug}-background-color` sets `--kitaris-auto-raised-color` and
  `--kitaris-auto-raised-text-color` (the raised colour's own text pair). `$page` sets them
  on `:root` for cards outside any picked background.
- A colour without `raised` gets the background with 8% of its text colour mixed in.
- A Card is a surface: links and buttons inside it follow the card's colours.
- A colour picked on the block wins over the Card colour, and gets the same card padding.
- Without the pairs, a Card is a 6% tint of the text colour.
- Padding: `card.padding` (`--wp--custom--card--padding`, default `2rem`). Radius:
  `border-radius.tile`.

Blocks use the `card` mixin from `@siratik/ui-kit/components/surfaces`, at zero
specificity.

## Form Configuration

```json
{
	"version": 3,
	"settings": {
		"custom": {
			"form": {
				"field": {
					"spacing": "0.5rem",
					"padding-x": "0.75rem",
					"padding-y": "0.5rem",
					"height": "3rem",
					"border-width": "1px",
					"border-color": "rgba(0, 0, 0, 0.2)",
					"border-radius": "0.5rem",
					"background-color": "#fff",
					"color": "#000"
				},
				"label": {
					"font-size": "1em",
					"spacing": "0.5rem"
				},
				"choice": {
					"size": "1.125rem",
					"spacing": "0.5rem",
					"border-radius": "0.25rem",
					"checked-color": "var(--wp--preset--color--primary)"
				},
				"file": {
					"min-height": "8rem",
					"border-style": "dashed",
					"border-color": "rgba(0, 0, 0, 0.25)"
				}
			}
		}
	}
}
```

### Form fields (`form.field`)

Text inputs, selects and textareas.

| theme.json key | CSS variable | Default |
|---|---|---|
| `form.field.spacing` | `--wp--custom--form--field--spacing` | `0.5rem` (space between fields) |
| `form.field.padding-x` | `--wp--custom--form--field--padding-x` | `0.75rem` |
| `form.field.padding-y` | `--wp--custom--form--field--padding-y` | `0.5rem` |
| `form.field.height` | `--wp--custom--form--field--height` | `3rem` |
| `form.field.border-width` | `--wp--custom--form--field--border-width` | `border-width.tiny`, else `1px` |
| `form.field.border-color` | `--wp--custom--form--field--border-color` | `color.border-light`, else 12.5% of the text colour |
| `form.field.border-radius` | `--wp--custom--form--field--border-radius` | `0` |
| `form.field.background-color` | `--wp--custom--form--field--background-color` | the `base` colour preset, else `#fff` |
| `form.field.color` | `--wp--custom--form--field--color` | `currentColor` |

| `form.field.focus-color` | `--wp--custom--form--field--focus-color` | the field text colour |

The focus ring uses `form.field.focus-color` and the shared focus width (see Focus).

### Labels (`form.label`)

| theme.json key | CSS variable | Default |
|---|---|---|
| `form.label.font-size` | `--wp--custom--form--label--font-size` | `1em` |
| `form.label.spacing` | `--wp--custom--form--label--spacing` | `0.5rem` (between a label and its field) |

Help text uses `small-text.font-size` (`--wp--custom--small-text--font-size`), else `0.875em`.

### Checkboxes and radios (`form.choice`)

| theme.json key | CSS variable | Default |
|---|---|---|
| `form.choice.size` | `--wp--custom--form--choice--size` | `1.125rem` |
| `form.choice.spacing` | `--wp--custom--form--choice--spacing` | `0.44rem` (between a choice and its label) |
| `form.choice.border-radius` | `--wp--custom--form--choice--border-radius` | `0` (checkboxes; radios are round) |
| `form.choice.checked-color` | `--wp--custom--form--choice--checked-color` | `color.primary`, else the `contrast` preset |

### File inputs (`form.file`)

| theme.json key | CSS variable | Default |
|---|---|---|
| `form.file.min-height` | `--wp--custom--form--file--min-height` | `8rem` |
| `form.file.border-style` | `--wp--custom--form--file--border-style` | `dashed` |
| `form.file.border-color` | `--wp--custom--form--file--border-color` | `color.border-medium`, else 25% of the text colour |

### Rounded surfaces (textareas, choice cards)

Textareas and choice cards use the shared tile radius, so line inputs can stay
square while panels are rounded:

| theme.json key | CSS variable | Default |
|---|---|---|
| `border-radius.tile` | `--wp--custom--border-radius--tile` | `0` |

## Testing Your Configuration

After updating theme.json:

1. **Clear caches** (page cache, if any)
2. **Inspect a form field** in your browser's DevTools
3. **Check the computed styles**: you should see your values

```css
input {
	padding: var(--wp--custom--form--field--padding-y, 0.5rem) var(--wp--custom--form--field--padding-x, 0.75rem);
	border-radius: var(--wp--custom--form--field--border-radius, 0);
}
```

## Advanced: CSS Override

You can also set the custom properties in CSS:

```css
/* Site-wide */
:root {
	--wp--custom--form--field--padding-x: 1rem;
	--wp--custom--form--field--border-radius: 0.75rem;
}

/* Or one form */
.my-special-form {
	--wp--custom--form--field--padding-x: 1.5rem;
}
```

## Why This Works Across Plugins

```
┌───────────────────────────────────────┐
│ Theme theme.json                       │
│ Defines: --wp--custom--form--field--… │
└──────────────┬────────────────────────┘
               │
               │ CSS variables cascade globally
               │
       ┌───────┴───────┬────────────────┬──────────────┐
       │               │                │              │
┌──────▼──────┐ ┌──────▼──────┐ ┌──────▼──────┐ ┌────▼─────┐
│ kitaris-    │ │ kitaris-    │ │ kitaris-    │ │ compass  │
│ forms       │ │ blocks      │ │ directory   │ │ theme    │
│ (reads var) │ │ (reads var) │ │ (reads var) │ │(reads var│
└─────────────┘ └─────────────┘ └─────────────┘ └──────────┘
```

All packages read the **same CSS variables**, defined once in your theme.

## Migration from Hardcoded Values

**Before:**

```scss
input {
	padding: 1rem; // Hardcoded in plugin
}
```

**After (using ui-kit):**

```scss
@use "@siratik/ui-kit/forms" as *;
// Reads the theme.json values automatically
```

No configuration needed in the plugin: the theme controls everything.
