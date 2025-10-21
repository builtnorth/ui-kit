# @builtnorth/ui-kit

NPM package for very minimal global and common CSS and JS utilities.

## Installation

```bash
npm install @builtnorth/ui-kit
```

## Usage

This package exports SCSS files that can be imported into your project using modern `@use` syntax.

### Import Everything

```scss
@use "@builtnorth/ui-kit" as *;
```

### Configure Once in Your Theme (Recommended)

Configure ui-kit in your theme's `theme.json` - applies to ALL plugins automatically:

```json
{
	"version": 3,
	"settings": {
		"custom": {
			"spacing": {
				"form-field": "1rem"
			},
			"border-radius": {
				"form-field": "0.5rem"
			}
		}
	}
}
```

**✨ Theme configuration applies everywhere** - no need to configure each plugin separately!

**📚 See [THEME-CONFIGURATION.md](./THEME-CONFIGURATION.md) for the complete theme.json guide.**

### Advanced: SCSS Variable Override (Per-Package)

For package-specific overrides:

```scss
@use "@builtnorth/ui-kit" with (
	$form-field-padding: 1rem,
	$form-field-border-radius: 0.5rem
);
```

**📚 See [CONFIGURATION.md](./CONFIGURATION.md) for SCSS configuration details.**

### Import Essential Styles Only

```scss
@use "@builtnorth/ui-kit/essential-styles" as *;
```

### Import Specific Modules

```scss
// Base styles
@use "@builtnorth/ui-kit/base" as *;
@use "@builtnorth/ui-kit/base/utilities" as *;
@use "@builtnorth/ui-kit/base/reset" as *;
@use "@builtnorth/ui-kit/base/accessibility" as *;
@use "@builtnorth/ui-kit/base/images" as *;
@use "@builtnorth/ui-kit/base/colors" as *;

// Components (with namespace)
@use "@builtnorth/ui-kit/components" as ui;
@use "@builtnorth/ui-kit/components/buttons" as ui;
@use "@builtnorth/ui-kit/components/slider" as ui;

// Forms
@use "@builtnorth/ui-kit/forms" as *;
@use "@builtnorth/ui-kit/forms/label" as *;
@use "@builtnorth/ui-kit/forms/fieldset" as *;
@use "@builtnorth/ui-kit/forms/select" as *;
@use "@builtnorth/ui-kit/forms/checkbox" as *;
@use "@builtnorth/ui-kit/forms/text" as *;

// Helpers (commonly namespaced as 'ui')
@use "@builtnorth/ui-kit/helpers" as ui;
@use "@builtnorth/ui-kit/helpers/mixins" as ui;
@use "@builtnorth/ui-kit/helpers/functions" as ui;
@use "@builtnorth/ui-kit/helpers/media-queries" as *;

// Layout
@use "@builtnorth/ui-kit/layout" as *;
@use "@builtnorth/ui-kit/layout/grid" as grid;
@use "@builtnorth/ui-kit/layout/columns" as *;

// Gutenberg
@use "@builtnorth/ui-kit/gutenberg" as *;
@use "@builtnorth/ui-kit/gutenberg/quirks" as *;
@use "@builtnorth/ui-kit/gutenberg/placeholders" as *;
@use "@builtnorth/ui-kit/gutenberg/appenders" as *;
```

> **Note:** The `@use` syntax is recommended over the deprecated `@import`. Use `as *` to include styles globally, or use `as ui` (or any namespace) to access mixins and functions with a namespace prefix.

## What's Included

- **Base**: Reset, utilities, accessibility, images, and color utilities
- **Components**: Buttons, sliders, pagination
- **Forms**: Styled form elements (labels, fieldsets, selects, checkboxes, text inputs)
- **Helpers**: Mixins, functions, and media query utilities
- **Layout**: Grid and column systems
- **Gutenberg**: WordPress block editor specific styles

## License

GPL-2.0-or-later

## Author

Built North
