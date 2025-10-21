# UI Kit Theme Configuration (WordPress theme.json)

## Overview

UI Kit uses **CSS custom properties** that map to WordPress theme.json settings. This means:

✅ **Configure ONCE in your theme** → applies to ALL plugins using ui-kit  
✅ **Runtime configuration** → no need to recompile plugins  
✅ **Theme always wins** → centralized control

## How It Works

1. Your **theme** defines values in `theme.json`
2. WordPress outputs these as CSS custom properties (CSS variables)
3. **ALL plugins** using ui-kit read from these same CSS variables
4. Changes in theme.json **instantly apply everywhere**

## Complete theme.json Configuration

Add this to your theme's `theme.json`:

```json
{
	"version": 3,
	"settings": {
		"custom": {
			"spacing": {
				"form-field": "1rem",
				"form-column-gap": "1.5rem",
				"form-choice-gap": "0.75rem"
			},
			"border-width": {
				"form-field": "2px"
			},
			"border-radius": {
				"form-field": "0.5rem",
				"form-choice": "0.375rem"
			},
			"color": {
				"form-field-border": "rgba(0, 0, 0, 0.2)"
			},
			"form-choice-size": "1.25rem"
		}
	}
}
```

## Available Configuration Options

### Form Fields

Configure text inputs, selects, and textareas:

```json
{
	"settings": {
		"custom": {
			"spacing": {
				"form-field": "1rem" // Padding inside form fields
			},
			"border-width": {
				"form-field": "2px" // Border thickness
			},
			"border-radius": {
				"form-field": "0.5rem" // Rounded corners
			},
			"color": {
				"form-field-border": "#e0e0e0" // Border color
			}
		}
	}
}
```

**CSS Variables Created:**

- `--wp--custom--spacing--form-field`
- `--wp--custom--border-width--form-field`
- `--wp--custom--border-radius--form-field`
- `--wp--custom--color--form-field-border`

### Checkboxes & Radio Buttons

```json
{
	"settings": {
		"custom": {
			"form-choice-size": "1.5rem", // Size of checkbox/radio
			"border-radius": {
				"form-choice": "0.5rem" // Rounded corners for checkbox
			},
			"spacing": {
				"form-choice-gap": "1rem" // Gap between choice and label
			}
		}
	}
}
```

**CSS Variables Created:**

- `--wp--custom--form-choice-size`
- `--wp--custom--border-radius--form-choice`
- `--wp--custom--spacing--form-choice-gap`

### Colors

Use WordPress color presets for consistency:

```json
{
	"settings": {
		"color": {
			"palette": [
				{
					"slug": "primary",
					"color": "#0066cc",
					"name": "Primary"
				}
			]
		}
	}
}
```

**CSS Variables Created:**

- `--wp--preset--color--primary` (used for focus states, checked states)

### Spacing Scale

Define your spacing system once:

```json
{
	"settings": {
		"spacing": {
			"spacingSizes": [
				{
					"slug": "20",
					"size": "0.5rem",
					"name": "X-Small"
				},
				{
					"slug": "30",
					"size": "0.75rem",
					"name": "Small"
				},
				{
					"slug": "40",
					"size": "1rem",
					"name": "Medium"
				}
			]
		}
	}
}
```

**CSS Variables Created:**

- `--wp--preset--spacing--20`
- `--wp--preset--spacing--30`
- `--wp--preset--spacing--40`

## Real-World Example

Here's a complete theme.json with form customization:

```json
{
	"version": 3,
	"settings": {
		"color": {
			"palette": [
				{
					"slug": "primary",
					"color": "#2563eb",
					"name": "Primary Blue"
				},
				{
					"slug": "white",
					"color": "#ffffff",
					"name": "White"
				}
			]
		},
		"spacing": {
			"spacingSizes": [
				{
					"slug": "20",
					"size": "0.5rem",
					"name": "2X-Small"
				},
				{
					"slug": "30",
					"size": "0.75rem",
					"name": "X-Small"
				},
				{
					"slug": "40",
					"size": "1rem",
					"name": "Small"
				},
				{
					"slug": "50",
					"size": "1.5rem",
					"name": "Medium"
				}
			]
		},
		"custom": {
			"spacing": {
				"form-field": "1rem",
				"form-column-gap": "1.5rem",
				"form-choice-gap": "0.75rem"
			},
			"border-width": {
				"form-field": "1px"
			},
			"border-radius": {
				"form-field": "0.5rem",
				"form-choice": "0.375rem"
			},
			"color": {
				"form-field-border": "#d1d5db"
			},
			"form-choice-size": "1.25rem"
		}
	}
}
```

## Testing Your Configuration

After updating theme.json:

1. **Clear cache** (if using caching)
2. **Inspect a form field** in browser DevTools
3. **Check computed styles** - you should see your custom property values

Example in DevTools:

```css
input {
	padding: var(--wp--custom--spacing--form-field, 0.11rem); /* Your value! */
	border-width: var(--wp--custom--border-width--form-field, 1px);
}
```

## Advanced: CSS Override

You can also override in custom CSS if needed:

```css
/* In your theme's style.css or custom CSS */
:root {
	--wp--custom--spacing--form-field: 1.25rem;
	--wp--custom--border-radius--form-field: 0.75rem;
}

/* Or scope to specific forms */
.my-special-form {
	--wp--custom--spacing--form-field: 2rem;
}
```

## Why This Works Across Plugins

```
┌─────────────────────────────────────┐
│ Theme theme.json                     │
│ Defines: --wp--custom--spacing--... │
└──────────────┬──────────────────────┘
               │
               │ CSS Variables cascade globally
               │
       ┌───────┴───────┬────────────────┬──────────────┐
       │               │                │              │
┌──────▼──────┐ ┌──────▼──────┐ ┌──────▼──────┐ ┌────▼─────┐
│ polaris-    │ │ polaris-    │ │ polaris-    │ │ compass  │
│ forms       │ │ blocks      │ │ directory   │ │ theme    │
│ (reads var) │ │ (reads var) │ │ (reads var) │ │(reads var│
└─────────────┘ └─────────────┘ └─────────────┘ └──────────┘
```

All packages read from the **same CSS variables** defined once in your theme!

## Migration from Hardcoded Values

If you have plugins with hardcoded form styles:

**Before:**

```scss
input {
	padding: 1rem; // Hardcoded in plugin
}
```

**After (using ui-kit):**

```scss
@use "@builtnorth/ui-kit/forms" as *;
// Automatically uses theme.json values!
```

No configuration needed in the plugin - theme controls everything! 🎉
