# UI Kit Configuration Guide

## Overview

UI Kit can be customized in three ways:

1. **WordPress theme.json** - Runtime customization via CSS custom properties
2. **SCSS Variables** - Compile-time customization (this guide)
3. **Direct overrides** - Write your own CSS to override defaults

## Method 1: Using SCSS with `@use` (Recommended)

The modern Sass `@use` syntax allows you to configure variables when importing:

```scss
// In your theme or plugin SCSS file
@use "@builtnorth/ui-kit" with (
	$form-field-padding: 1rem,
	$form-field-border-radius: 0.5rem,
	$button-padding-y: 1rem,
	$button-padding-x: 2rem,
	$breakpoint-md: 800px
);
```

## Method 2: Define Variables Before Import (Legacy)

If using `@import`, define variables before importing:

```scss
// Your custom config
$form-field-padding: 1rem;
$form-field-border-radius: 0.5rem;
$button-padding-y: 1rem;

// Then import ui-kit
@import "@builtnorth/ui-kit";
```

## Method 3: Import Config Separately

You can also just import the config to use variables in your own code:

```scss
@use "@builtnorth/ui-kit/config" as ui-kit;

.my-custom-button {
	padding: ui-kit.$button-padding-y ui-kit.$button-padding-x;
	border-radius: ui-kit.$radius-md;
}
```

## Available Configuration Variables

### Form Field Settings

```scss
$form-field-padding: 0.11rem;
$form-field-border-width: 1px;
$form-field-border-color: #8e8e8e;
$form-field-border-radius: 0;
$form-field-focus-color: var(--wp--preset--color--primary);
```

### Form Choice Settings (Checkbox/Radio)

```scss
$form-choice-size: 1.125rem;
$form-choice-spacing: 0.25rem;
$form-choice-border-radius: 0.25rem;
$form-choice-bg-color: #fff;
$form-choice-checked-bg: var(--wp--preset--color--primary);
$form-choice-checked-border: var(--wp--preset--color--primary);
```

### Button Settings

```scss
$button-padding-y: 0.75rem;
$button-padding-x: 1.5rem;
$button-border-radius: 0.25rem;
$button-font-weight: 500;
```

### Layout Settings

```scss
$grid-gutter: 1rem;
$grid-columns: 12;
$container-max-width: 1200px;
```

### Breakpoints

```scss
$breakpoint-sm: 576px;
$breakpoint-md: 768px;
$breakpoint-lg: 992px;
$breakpoint-xl: 1200px;
$breakpoint-xxl: 1400px;
```

### Spacing Scale

```scss
$spacing-20: 0.25rem;
$spacing-30: 0.5rem;
$spacing-40: 1rem;
$spacing-50: 1.5rem;
$spacing-60: 2rem;
$spacing-70: 3rem;
```

### Border Radius Scale

```scss
$radius-sm: 0.25rem;
$radius-md: 0.5rem;
$radius-lg: 1rem;
$radius-full: 9999px;
```

### Transitions

```scss
$transition-base: all 0.2s ease-in-out;
$transition-fast: all 0.1s ease-in-out;
$transition-slow: all 0.3s ease-in-out;
```

### Z-Index Scale

```scss
$z-index-dropdown: 1000;
$z-index-sticky: 1020;
$z-index-fixed: 1030;
$z-index-modal-backdrop: 1040;
$z-index-modal: 1050;
$z-index-popover: 1060;
$z-index-tooltip: 1070;
```

## Real-World Examples

### Example 1: Customize Forms for Your Brand

```scss
// theme.scss
@use "@builtnorth/ui-kit" with (
	// Bigger, rounder form fields
	$form-field-padding: 1rem,
	$form-field-border-radius: 0.5rem,
	$form-field-border-width: 2px,

	// Larger checkboxes
	$form-choice-size: 1.5rem,
	$form-choice-border-radius: 0.5rem
);
```

### Example 2: Adjust Breakpoints for Your Design

```scss
@use "@builtnorth/ui-kit" with (
	// Custom breakpoints to match your design
	$breakpoint-sm: 640px,
	$breakpoint-md: 768px,
	$breakpoint-lg: 1024px,
	$breakpoint-xl: 1280px
);
```

### Example 3: Import Only What You Need

```scss
// Only import forms with custom config
@use "@builtnorth/ui-kit/forms" with (
	$form-field-padding: 1rem
);

// Or import specific components
@use "@builtnorth/ui-kit/forms/text" with (
	$form-field-padding: 1rem
);
@use "@builtnorth/ui-kit/forms/checkbox" with (
	$form-choice-size: 1.5rem
);
```

## Combining with WordPress theme.json

UI Kit variables can use CSS custom properties, so you get the best of both worlds:

**SCSS** (compile-time, can't change):

```scss
@use "@builtnorth/ui-kit" with (
	$form-field-padding: 1rem // Fixed at compile time
);
```

**theme.json** (runtime, can change):

```json
{
	"settings": {
		"custom": {
			"spacing": {
				"form-field": "1rem" // Can be changed by user/theme
			}
		}
	}
}
```

UI Kit defaults to theme.json values but lets you override with SCSS when needed!

## Migration Guide

If you're already using ui-kit and want to start customizing:

1. **Identify** what you want to customize
2. **Find** the variable name in `_config.scss`
3. **Override** using `@use` with the new value
4. **Test** and verify changes compile correctly

No breaking changes - all existing code continues to work!
