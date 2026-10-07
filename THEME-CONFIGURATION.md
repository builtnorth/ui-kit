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

The focus outline uses `color.focus` (`--wp--custom--color--focus`), else `currentColor`.

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
│ polaris-    │ │ polaris-    │ │ polaris-    │ │ compass  │
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
@use "@builtnorth/ui-kit/forms" as *;
// Reads the theme.json values automatically
```

No configuration needed in the plugin: the theme controls everything.
