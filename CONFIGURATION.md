# UI Kit Configuration Guide

## Overview

UI Kit can be customized in three ways:

1. **WordPress theme.json** - Runtime customization via CSS custom properties
2. **SCSS Variables** - Compile-time customization (this guide)
3. **Direct overrides** - Write your own CSS to override defaults

## Method 1: Configure the config module with `@use ... with`

Every variable lives in one module, `@builtnorth/ui-kit/config`. Configure it
**before any other ui-kit `@use`** in the same stylesheet: Sass configures a
module only the first time it loads, and every ui-kit module then reads the
configured values.

```scss
// In your theme or plugin SCSS file
@use "@builtnorth/ui-kit/config" with (
	$form-field-padding-x: 1rem,
	$form-field-border-radius: 0.5rem,
	$breakpoint-md: 800px
);
@use "@builtnorth/ui-kit";
```

`@use "@builtnorth/ui-kit" with (...)` does not work: the entry point doesn't
forward the config variables, so Sass reports "This variable was not declared
with !default in the @used module".

Global variables before an `@import` don't configure ui-kit either, because its
modules load the config with `@use`.

## Method 2: Import Config Separately

You can also just import the config to use variables in your own code:

```scss
@use "@builtnorth/ui-kit/config" as ui-kit;

.my-custom-button {
	padding: ui-kit.$button-padding-y ui-kit.$button-padding-x;
	border-radius: ui-kit.$radius-md;
}
```

## Available Configuration Variables

Every variable and its default is in
[`src/scss/_config.scss`](./src/scss/_config.scss). Most defaults read a
theme.json custom property first, with a literal fallback, for example:

```scss
$form-field-padding-x: var(--wp--custom--form--field--padding-x, 0.75rem) !default;
```

The groups are:

- **Form fields**: `$form-field-spacing`, `$form-field-padding-x`, `$form-field-padding-y`, `$form-field-height`, `$form-field-border-width`, `$form-field-border-color`, `$form-field-border-radius`, `$form-field-background-color`, `$form-field-color`, `$form-field-focus-color`
- **Form labels**: `$form-label-text-size`, `$form-label-spacing`, `$form-label-help-text-size`
- **Form surfaces** (textareas, choice cards): `$form-surface-border-radius` (theme.json `settings.custom.form.surface.border-radius`)
- **Form choices** (checkbox/radio): `$form-choice-size`, `$form-choice-spacing`, `$form-choice-border-radius`, `$form-choice-checked-color`
- **File inputs**: `$form-file-min-height`, `$form-file-padding`, `$form-file-border-width`, `$form-file-border-style`, `$form-file-border-color`
- **Buttons**: `$button-padding-y`, `$button-padding-x`, `$button-border-radius`, `$button-font-weight`
- **Layout**: `$grid-gutter`, `$grid-columns`, `$container-max-width`
- **Breakpoints**: `$breakpoint-xxs` (320px), `$breakpoint-xs` (480px), `$breakpoint-sm` (640px), `$breakpoint-md` (768px), `$breakpoint-lg` (1024px), `$breakpoint-xl` (1280px), `$breakpoint-xxl` (1536px). They drive the `gt-*`/`lt-*`/`only-*` media query mixins.
- **Colours**: `$color-primary`, `$color-secondary`, `$color-base`, `$color-contrast`
- **Spacing scale**: `$spacing-20` to `$spacing-70`
- **Border radius scale**: `$radius-sm`, `$radius-md`, `$radius-lg`, `$radius-full`
- **Transitions**: `$transition-duration-fast`, `$transition-duration-base`, `$transition-duration-slow`, `$transition-easing-default`, `$transition-base`, `$transition-fast`, `$transition-slow`

## Real-World Examples

### Example 1: Customize Forms for Your Brand

```scss
// theme.scss
@use "@builtnorth/ui-kit/config" with (
	// Bigger, rounder form fields
	$form-field-padding-x: 1rem,
	$form-field-padding-y: 0.75rem,
	$form-field-border-radius: 0.5rem,
	$form-field-border-width: 2px,

	// Larger checkboxes
	$form-choice-size: 1.5rem,
	$form-choice-border-radius: 0.5rem
);
@use "@builtnorth/ui-kit";
```

### Example 2: Adjust Breakpoints for Your Design

```scss
@use "@builtnorth/ui-kit/config" with (
	// Custom breakpoints to match your design
	$breakpoint-sm: 600px,
	$breakpoint-md: 800px,
	$breakpoint-lg: 1100px,
	$breakpoint-xl: 1300px
);
@use "@builtnorth/ui-kit";
```

### Example 3: Import Only What You Need

```scss
@use "@builtnorth/ui-kit/config" with (
	$form-field-padding-x: 1rem,
	$form-choice-size: 1.5rem
);

// Only the form styles, with the config above
@use "@builtnorth/ui-kit/forms";

// Or specific form modules
@use "@builtnorth/ui-kit/forms/shared";
@use "@builtnorth/ui-kit/forms/checkbox";
```

## Combining with WordPress theme.json

UI Kit variables can use CSS custom properties, so you get the best of both worlds:

**SCSS** (compile-time, can't change):

```scss
@use "@builtnorth/ui-kit/config" with (
	$form-field-padding-x: 1rem // Fixed at compile time
);
```

**theme.json** (runtime, can change):

```json
{
	"settings": {
		"custom": {
			"form": {
				"field": {
					"padding-x": "1rem"
				}
			}
		}
	}
}
```

A configured SCSS value replaces the whole default, including its theme.json
custom property. Leave a variable unconfigured to keep reading theme.json.

## Migration Guide

If you're already using ui-kit and want to start customizing:

1. **Identify** what you want to customize
2. **Find** the variable name in `_config.scss`
3. **Override** it by configuring `@builtnorth/ui-kit/config` with the new value, before your other ui-kit `@use` rules
4. **Test** and verify changes compile correctly
