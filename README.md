# @builtnorth/ui-kit

NPM package for very minimal global and common CSS and JS utilities.

## Installation

```bash
npm install @builtnorth/ui-kit
```

## Usage

This package exports SCSS files that can be imported into your project.

### Import Everything

```scss
@import "@builtnorth/ui-kit";
```

### Import Essential Styles Only

```scss
@import "@builtnorth/ui-kit/essential-styles";
```

### Import Specific Modules

```scss
// Base styles
@import "@builtnorth/ui-kit/base";
@import "@builtnorth/ui-kit/base/utilities";
@import "@builtnorth/ui-kit/base/reset";
@import "@builtnorth/ui-kit/base/accessibility";
@import "@builtnorth/ui-kit/base/images";
@import "@builtnorth/ui-kit/base/colors";

// Components
@import "@builtnorth/ui-kit/components";
@import "@builtnorth/ui-kit/components/buttons";
@import "@builtnorth/ui-kit/components/slider";

// Forms
@import "@builtnorth/ui-kit/forms";
@import "@builtnorth/ui-kit/forms/label";
@import "@builtnorth/ui-kit/forms/fieldset";
@import "@builtnorth/ui-kit/forms/select";
@import "@builtnorth/ui-kit/forms/checkbox";
@import "@builtnorth/ui-kit/forms/text";

// Helpers
@import "@builtnorth/ui-kit/helpers";
@import "@builtnorth/ui-kit/helpers/mixins";
@import "@builtnorth/ui-kit/helpers/functions";
@import "@builtnorth/ui-kit/helpers/media-queries";

// Layout
@import "@builtnorth/ui-kit/layout";
@import "@builtnorth/ui-kit/layout/grid";
@import "@builtnorth/ui-kit/layout/columns";

// Gutenberg
@import "@builtnorth/ui-kit/gutenberg";
@import "@builtnorth/ui-kit/gutenberg/quirks";
@import "@builtnorth/ui-kit/gutenberg/placeholders";
@import "@builtnorth/ui-kit/gutenberg/appenders";
```

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
