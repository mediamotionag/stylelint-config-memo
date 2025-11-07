# Stylelint Configuration Documentation

## Table of Contents
- [Base Configuration](#base-configuration)
- [Stylistic Rules](#stylistic-rules)
- [At-Rule Configuration](#at-rule-configuration)
- [Naming Conventions](#naming-conventions)
- [Complexity & Specificity Limits](#complexity--specificity-limits)
- [Custom Properties & Media Queries](#custom-properties--media-queries)
- [Warnings & Overrides](#warnings--overrides)
- [Code Quality](#code-quality)
- [SCSS Specific Rules](#scss-specific-rules)
- [Property Ordering](#property-ordering)

---

## Base Configuration

```javascript
'extends': ['stylelint-config-standard-scss'],
'plugins': [
    '@stylistic/stylelint-plugin',
    'stylelint-order',
]
```

**Extends:** `stylelint-config-standard-scss@16.0.0` which includes:
- `stylelint-config-recommended` - Core CSS linting rules
- `stylelint-config-recommended-scss` - SCSS-specific rules
- `stylelint-config-standard` - Standard stylistic rules

**Plugins:**
- `@stylistic/stylelint-plugin` - Official stylistic rules plugin
- `stylelint-order` - Property ordering and grouping rules

---

## Stylistic Rules

### @stylistic/color-hex-case: 'upper'`
Enforces UPPERCASE for hex colors (`#FFF` instead of `#fff`). Makes colors more visually distinct in code.

### @stylistic/at-rule-semicolon-space-before: 'never'`
No space before semicolon in @rules (`@include foo;` not `@include foo ;`).

### @stylistic/at-rule-name-space-after: 'always'`
Requires space after @rule name (`@media ` not `@media`).

### @stylistic/block-closing-brace-newline-after: 'always'` (with exceptions)
Requires newline after closing brace, except for SCSS control structures (`if`, `else`, `elseif`). This keeps SCSS conditionals compact.

### @stylistic/declaration-colon-newline-after: 'always-multi-line'`
For multi-line declarations, require newline after colon for better readability.

### @stylistic/function-comma-newline-before: 'never-multi-line'`
Disallows newline before function commas in multi-line functions.

### @stylistic/max-line-length: 160`
Maximum line length of 160 characters. Balance between readability and avoiding excessive line breaks.

### @stylistic/media-query-list-comma-newline-before: 'never-multi-line'`
Keeps media query lists compact.

### @stylistic/selector-list-comma-newline-before: 'never-multi-line'`
Keeps selector lists compact.

### @stylistic/selector-list-comma-space-after: 'always-single-line'`
Requires space after commas in single-line selector lists (`.a, .b, .c`).

### @stylistic/string-quotes: 'single'`
Enforces single quotes for strings. Consistent with JavaScript conventions.

### @stylistic/indentation: 4`
Uses 4 spaces for indentation. Project standard.

### @stylistic/media-feature-parentheses-space-inside: 'never'`
No space inside media feature parentheses: `@media (min-width: 768px)` not `( min-width: 768px )`.

### @stylistic/number-leading-zero: 'never'`
No leading zero for decimal numbers (`.5` instead of `0.5`). Shorter and common convention.

---

## At-Rule Configuration

### `at-rule-empty-line-before`
Requires empty line before @rules for visual separation, with exceptions:
- After comments
- SCSS control structures (`@extend`, `@include`, `@else`, `@elseif`, `@content`)

### `at-rule-no-unknown`
Allows SCSS and modern CSS @-rules that would otherwise be flagged:
- **SCSS**: `@content`, `@extend`, `@include`, `@mixin`, `@if`, `@for`, `@each`, `@function`, `@return`, `@use`, `@forward`, `@while`, `@at-root`, `@else`, `@elseif`, `@error`
- **Modern CSS**: `@container` (Container Queries)

---

## Naming Conventions

### `function-name-case`
Allows camelCase in CSS/SCSS function names while requiring lowercase start (`myFunction`, `calculateSize`).

### `font-weight-notation: 'numeric'`
Use numeric font weights (400, 700) but allows relative keywords (`bolder`, `lighter`).

### `value-keyword-case: 'lower'`
Enforces lowercase for keyword values (`red`, `solid`, `auto`).

### `selector-class-pattern: '^[a-z][a-z-A-Z_0-9]*$'`
Allows:
- camelCase: `.myComponent`
- underscores: `.my_class`
- BEM: `.myComponent__element`, `.myComponent--modifier`

### `custom-media-pattern: '^[a-z][a-z-A-Z0-9]*$'`
Allows camelCase in custom media queries (`@media (--myBreakpoint)`).

### `custom-property-pattern: '^[a-z][a-z-A-Z0-9]*(--[a-z0-9-]+)*$'`
Allows BEM-style double dashes in CSS custom properties:
- `--color-primary--1`
- `--spacing--lg`
- `--font-size--body`

---

## Complexity & Specificity Limits

### `selector-max-compound-selectors: 5` (warning)
Limits compound selectors (`.a .b .c .d .e`). Too many nested selectors indicate overly specific CSS.

### `selector-max-id: 1`
Limits to 1 ID selector per selector. IDs have very high specificity and should be used sparingly.

### `selector-max-type: 3` (warning)
Limits to 3 type selectors (`div`, `span`, `p`). Encourages using classes instead of type selectors.

### `selector-max-combinators: 5` (warning)
Limits to 5 combinators (`>`, `+`, `~`, space). Prevents overly complex selectors.

### `selector-max-universal: 1`
Limits to 1 universal selector (`*`). Universal selectors impact performance.

### `selector-max-specificity: '1,3,0'`
Maximum specificity: 1 ID, 3 classes, unlimited elements. Helps maintain manageable specificity.

---

## Warnings & Overrides

### `no-descending-specificity: null`
**Disabled**. This rule has too many false positives in component-based CSS where specificity often decreases intentionally.

### `no-duplicate-selectors` (warning)
Makes duplicate selectors a warning instead of error. Sometimes duplicate selectors are intentional for organization.

---

## Code Quality

### `declaration-empty-line-before`
Requires empty line before declarations for visual grouping, except:
- After comments
- After other declarations (keeps related properties together)
- First nested declaration

### `selector-attribute-quotes: 'always'`
Always quote attribute selectors: `[type="text"]` not `[type=text]`.

### `color-hex-length: 'short'`
Use short hex colors (`#FFF` instead of `#FFFFFF`). Shorter and equally clear.

### `rule-empty-line-before`
Requires empty line before rules for visual separation, except:
- After comments
- First nested rule

### `shorthand-property-no-redundant-values: true`
Disallows redundant values in shorthand properties:
- ❌ `margin: 10px 10px;`
- ✅ `margin: 10px;`

---

## SCSS Specific Rules

### `scss/dollar-variable-pattern: '^[a-z][a-z-A-Z_0-9]*$'`
Allows camelCase and underscores in SCSS variables:
- `$myVariable`
- `$my_var`
- `$myVar__modifier`

### `scss/percent-placeholder-pattern: '^[a-z][a-z-A-Z0-9]*$'`
Allows camelCase in SCSS placeholders: `%myPlaceholder`

### `scss/at-function-pattern: '^[a-z][a-z-A-Z0-9]*$'`
Allows camelCase in SCSS functions: `@function myFunc`

### `scss/declaration-nested-properties: 'never'`
Disallows nested properties syntax:
```scss
// ❌ Disallowed
font: {
  size: 10px;
  weight: 700;
}

// ✅ Preferred
font-size: 10px;
font-weight: 700;
```

---

## Property Ordering

Our property ordering follows a logical flow from **outer to inner**, **layout to visual**, and **structure to decoration**.

### Order Overview

```javascript
'order/order': [
    'dollar-variables',      // SCSS variables
    'custom-properties',     // CSS custom properties
    'declarations',          // Regular CSS properties
]
```

### Property Groups (in order)

#### 1. Variables
- `$variables` and `--custom-properties` always come first
- Keeps configuration at the top of each rule

#### 2. Box Sizing
- `box-sizing` - Determines how box model is calculated

#### 3. Content / Display
- `content` - Generated content
- `counter-*` - Counter manipulation
- `display` - Element display mode
- `container-*` - Container query configuration

#### 4. Legacy Layout (Float/Clear)
- `float`, `clear` - Legacy layout methods

#### 5. Inline Alignment
- `vertical-align` - Vertical alignment for inline elements

#### 6. Overflow
- `overflow-*` - How content overflow is handled
- Includes logical properties (`overflow-wrap`, `overflow-anchor`)

#### 7. Position
- `position` - Positioning scheme
- `inset-*` - Modern positioning shorthands
- `top`, `right`, `bottom`, `left` - Physical positioning
- `z-index` - Stacking order

#### 8. Grid Layout
- All `grid-*` properties
- `gap`, `row-gap`, `column-gap` - Grid spacing
- `subgrid` - CSS Subgrid

#### 9. Flexbox Layout
- All `flex-*` properties
- `justify-*`, `align-*`, `place-*` - Alignment
- `order` - Visual order
- `gap` - Flexbox spacing

#### 10. Text Columns
- `columns`, `column-*` - Multi-column text layout

#### 11. Table
- Table-specific properties (`table-layout`, `border-collapse`, etc.)

#### 12. List
- List styling (`list-style-*`)

#### 13. Box Size (Dimensions)
- `width`, `height`, `min-*`, `max-*`
- Logical properties (`block-size`, `inline-size`)
- `aspect-ratio` - Modern aspect ratio control

#### 14. Margin (Outer Spacing)
- Physical margins (`margin-top`, etc.)
- Logical margins (`margin-block-*`, `margin-inline-*`)

#### 15. Padding (Inner Spacing)
- Physical padding (`padding-top`, etc.)
- Logical padding (`padding-block-*`, `padding-inline-*`)

#### 16. Border
- All border properties (width, style, color, radius)
- Includes logical properties (`border-block-*`, `border-inline-*`)
- Logical border radius (`border-start-start-radius`, etc.)

#### 17. SVG Stroke/Fill
- SVG-specific properties (`stroke`, `fill`)

#### 18. Font
- All font-related properties
- `line-height`, `letter-spacing`

#### 19. Text
- Text styling and behavior
- Modern properties: `text-wrap-*`, `white-space-collapse`, `hyphenate-character`

#### 20. Color & Background
- `color` - Text color
- All `background-*` properties

#### 21. Outline
- Focus outline styling

#### 22. Visual Effects
- `box-shadow`, `opacity`, `visibility`
- Modern effects: `filter`, `backdrop-filter`, `mix-blend-mode`, `isolation`
- Transform properties

#### 23. Transitions & Animations
- All `transition-*` properties
- All `animation-*` properties
- Modern: `transition-behavior`, `animation-composition`, `animation-timeline`
- View Transitions: `view-transition-*`

#### 24. Advanced/Performance
- Performance: `content-visibility`, `contain-*`, `will-change`
- Scrolling: `scroll-behavior`, `scroll-snap-*`, `scroll-margin-*`, `scroll-padding-*`
- Scroll-driven animations: `scroll-timeline-*`
- Overscroll: `overscroll-behavior-*`
- Anchor positioning: `anchor-*`, `position-anchor`, `position-area`
- Interaction: `cursor`, `caret-color`, `accent-color`, `pointer-events`, `touch-action`, `user-select`
- Clipping: `clip`, `clip-path`
- Legacy: `zoom`

---

## Why This Ordering?

### 1. Follows Visual/Mental Model
Properties are ordered as you'd think about styling:
1. What is it? (display, positioning)
2. How big is it? (dimensions, spacing)
3. How does it look? (colors, borders, shadows)
4. How does it move? (transitions, animations)

### 2. Layout Before Decoration
Structural properties (display, position, dimensions) come before visual properties (colors, shadows).

### 3. Outer to Inner
- Position (where in document)
- Dimensions (how big)
- Margin (space outside)
- Border (edge)
- Padding (space inside)
- Content (colors, text)
