# Siratik UI Kit

NPM package for very minimal global and common CSS and JS utilities.

## Installation

```bash
npm install @siratik/ui-kit
```

## Usage

This package exports SCSS files that can be imported into your project using modern `@use` syntax.

### Import Everything

```scss
@use "@siratik/ui-kit" as *;
```

### Configure Once in Your Theme (Recommended)

Configure ui-kit in your theme's `theme.json` - applies to ALL plugins automatically:

```json
{
	"version": 3,
	"settings": {
		"custom": {
			"form": {
				"field": {
					"padding-x": "1rem",
					"border-radius": "0.5rem"
				}
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
// Configure the config module first, before any other ui-kit @use.
@use "@siratik/ui-kit/config" with (
	$form-field-padding-x: 1rem,
	$form-field-border-radius: 0.5rem
);
@use "@siratik/ui-kit";
```

**📚 See [CONFIGURATION.md](./CONFIGURATION.md) for SCSS configuration details.**

### Import Essential Styles Only

```scss
@use "@siratik/ui-kit/essential-styles" as *;
```

### Import Specific Modules

```scss
// Base styles
@use "@siratik/ui-kit/base" as *;
@use "@siratik/ui-kit/base/utilities" as *;
@use "@siratik/ui-kit/base/reset" as *;
@use "@siratik/ui-kit/base/accessibility" as *;
@use "@siratik/ui-kit/base/images" as *;
@use "@siratik/ui-kit/base/colors" as *;

// Components (with namespace)
@use "@siratik/ui-kit/components" as ui;
@use "@siratik/ui-kit/components/buttons" as ui;

// Buttons: mixins only (outputs no CSS), for a theme's button colours
@use "@siratik/ui-kit/components/button-mixins" as button;
// @include button.default-style;
// @include button.variant("primary", $background: ..., $text: ...);
// @include button.invert-on("primary", $variant: "primary");
@use "@siratik/ui-kit/components/slider" as ui;
@use "@siratik/ui-kit/components/lightbox" as *;
@use "@siratik/ui-kit/components/navigation" as *;
@use "@siratik/ui-kit/components/list-icon" as *;

// Forms: global element styles (every field on the page)
@use "@siratik/ui-kit/forms" as *;
@use "@siratik/ui-kit/forms/shared" as *;
@use "@siratik/ui-kit/forms/checkbox" as *;
@use "@siratik/ui-kit/forms/select" as *;
@use "@siratik/ui-kit/forms/file" as *;

// Forms: mixins, for fields inside your own wrapper
@use "@siratik/ui-kit/forms/mixins" as form;
// .my-filters select { @include form.form-field; @include form.form-select; }

// Helpers (commonly namespaced as 'ui')
@use "@siratik/ui-kit/helpers" as ui;
@use "@siratik/ui-kit/helpers/mixins" as ui;
@use "@siratik/ui-kit/helpers/functions" as ui;
@use "@siratik/ui-kit/helpers/media-queries" as *;

// Layout
@use "@siratik/ui-kit/layout" as *;
@use "@siratik/ui-kit/layout/grid" as grid;
@use "@siratik/ui-kit/layout/columns" as *;

// Gutenberg
@use "@siratik/ui-kit/gutenberg" as *;
@use "@siratik/ui-kit/gutenberg/quirks" as *;
@use "@siratik/ui-kit/gutenberg/placeholders" as *;
@use "@siratik/ui-kit/gutenberg/appenders" as *;
@use "@siratik/ui-kit/gutenberg/misc" as *;
@use "@siratik/ui-kit/gutenberg/quirks-editor" as *;
```

> **Note:** The `@use` syntax is recommended over the deprecated `@import`. Use `as *` to include styles globally, or use `as ui` (or any namespace) to access mixins and functions with a namespace prefix.

## What's Included

- **Base**: Reset, utilities, accessibility, images, and color utilities
- **Components**: Buttons, sliders, pagination
- **Forms**: Field chrome (labels, text inputs, selects, checkboxes, radios, range, file), as global element styles or as mixins you scope yourself
- **Helpers**: Mixins, functions, and media query utilities
- **Layout**: Grid and column systems
- **Gutenberg**: WordPress block editor specific styles

## Author

Built North

## License

GPL-3.0-or-later — see `LICENSE`.

## Disclaimer

This software is provided "as is", without warranty of any kind, express or
implied, including the warranties of merchantability and fitness for a
particular purpose. To the extent permitted by applicable law, the authors and
copyright holders, together with their parent, subsidiary, and affiliated
companies and contributors, are not liable for any direct, indirect,
incidental, or consequential damages arising from its use or inability to use
it, including loss of data, revenue, or site availability.

The software does not guarantee that your site is secure, ranks in search
results, or complies with privacy or other laws; that remains the site owner's
responsibility. AI-generated content can be inaccurate and should be reviewed
before publishing. Third-party services the software connects to are governed
by their own terms.

Keep a current backup and test changes on a staging site before applying them
to a live site. See sections 15 and 16 of the GNU General Public License,
version 3, for the full terms.
