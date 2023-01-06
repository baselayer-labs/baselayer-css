# Baselayer CSS

Baselayer CSS is a styling toolkit to facilitate the development of user interfaces.

## Getting started

To use Baselayer CSS, make sure you have the prerequisites installed before continuing on with the installation steps.

### Prerequisites

- [Node](https://nodejs.org/en/download/)
- [Dart Sass](https://sass-lang.com/install)

### Install via clone

Clone Baselayer CSS to the desired directory in your project. Remember that the path to the Baselayer module is affected depending on where you put it in relation to your `index.scss` file. The example below includes Baselayer CSS in the same directory as your `index.scss` file.

1. Clone the repository
   ```sh
   # Change to your desired directory
   cd my-project/styles/

   # Clone the project
   git clone https://github.com/baselayer-labs/baselayer-css
   ```
2. Include the Baselayer module in your `index.scss`
   ```scss
   // index.scss
   @use "baselayer" as *;
   ```
3. Configure Baselayer (optional but recommended)
   ```scss
   // index.scss
   @use "baselayer" as * with (
     $config: (
       responsive: true,
       theme: (
         breakpoints: (
           md: 40em,
           lg: 80em
         ),
         colors: (
           success: green,
           danger: red
         ),
         spacing: (
           nano: 0.25rem
         )
         ...
       ),
       layouts: (
         wrap: (
           max-width: 100rem
         )
       ),
       ...
     )
   );
   ```
4. Make sure your `index.scss` file compiles with a sass command
   ```js
   // package.json
   {
     "name": "my-project",
     "scripts": {
       "dev": "sass styles/index.scss public/index.css --no-source-map --style expanded",
       "build": "sass styles/index.scss public/index.min.css --no-source-map --style compressed",
       "watch": "npm run dev & npm run dev -- --watch"
     },
     "devDependencies": {
       "sass": "^1.43.2"
     }
   }
   ```  
## Core concepts

Will be updated soon.

## Layouts

### Flex

Flex handles box alignment and element composition with endless flexibility using one single class and custom properties.

```scss
// Default settings
.layout-flex {
  --flow: row; // flex-direction
  --wrap: wrap; // flex-wrap
  --align: normal; // align-items
  --place: normal; // place-content (align-content justify-content)
  --width: auto;
  --gap: 0;
}

.layout-flex > * {
  --basis: auto; // flex-basis
  --grow: 0; // flex-grow
  --shrink: 1; // flex-shrink
  --align: auto; // align-self
  --order: 0;
  --width: auto;
  --min-width: 0;
  --max-width: none;
}
```

```html
<!-- Align items along the center of the cross axis -->
<div class="layout-flex" style="--align: center;">
  <div>...</div>
  <div>...</div>
</div>

<!-- Align an item along the center of the cross axis -->
<div class="layout-flex">
  <div style="--align: center;">...</div>
  <div>...</div>
</div>

<!-- Pack rows in the center of the cross axis and justify items along the center of the main axis -->
<div class="layout-flex" style="--place: center;">
  <div>...</div>
  <div>...</div>
</div>

<!-- Pack rows at the start of the cross axis and justify items along the center of the main axis -->
<div class="layout-flex" style="--place: flex-start center;">
  <div>...</div>
  <div>...</div>
</div>
```

### Flow

Flow controls vertical spacing between elements within a container. That way we avoid the need to set margins directly on components. Instead, we style the context.

#### Class

`.layout-flow`

Variable | Default | Options | Breakpoints | Description
---|---|---|---|---
`--space` | `var(--space-medium)` | custom | `false` | Set the value for the `--space` variable, which is used to set the `margin-top` property on all child elements except the first one using the adjacent sibling combinator `> * + *`.

#### Usage

```html
<!-- Set a custom space flow between a set of elements -->
<div class="layout-flow" style="--space: 1rem;">
  <div>...</div>
  <div>...</div>
  <div>...</div>
  <div>...</div>
</div>
```

### Frame

Frame embeds a media object with a fixed aspect ratio and with the option to adjust the focus point and fit behavior.

#### Class

`.layout-frame`

Variable | Default | Options | Breakpoints | Description
---|---|---|---|---
`--object-x` | `50%` | custom | `true` | Set the x-axis value for the `object-position` property.
`--object-y` | `50%` | custom | `true` | Set the y-axis value for the `object-position` property.
`--object-fit` | `cover` | `contain`, `cover`, `fill`, `none`, `scale-down` | `true` | Set the `object-fit` property.
`--ratio` | `1/1` | `width/height` | `true` | Set the `spect-ratio` property.

#### Usage

```html
<!-- Display image in 1:1 format -->
<div class="layout-frame">
  <img src="..." alt="..." loading="lazy" />
</div>

<!-- Display image in 16:9 format and change to 5:4 format from the medium breakpoint -->
<div class="layout-frame" style="--ratio: 16/9; --md-ratio: 5/4;">
  <img src="..." alt="..." loading="lazy" />
</div>

<!-- Make sure the image keeps its proportions -->
<div class="layout-frame" style="--object-fit: contain;">
  <img src="..." alt="..." loading="lazy" />
</div>

<!-- Set focus point for the image -->
<div class="layout-frame" style="--object-x: 32%; --object-y: 45%;">
  <img src="..." alt="..." loading="lazy" />
</div>
```

### Grid

Build responsive grid systems with total layout freedom using one single class and custom properties.

```scss
// Default settings
.layout-grid {
  --flow: row; // grid-auto-flow
  --auto-cols: auto; // grid-auto-columns
  --auto-rows: auto; // grid-auto-rows
  --cols: 1; // grid-template-columns (repeat(var(--cols), var(--cols-size)))
  --cols-size: 1fr; // grid-template-columns (repeat(var(--cols), var(--cols-size)))
  --align: normal; // align-items
  --justify: normal; // justify-items
  --place: normal; // place-content (align-content justify-content)
  --gap: 0;
}

.layout-grid > * {
  --col: auto; // grid-column
  --row: auto; // grid-row
  --align: auto; // align-self
  --justify: auto; // justify-self
  --order: 0;
}
```

```html
<!-- A simple responsive grid from one column to four columns -->
<div
  class="layout-grid"
  style="--cols: 1; --sm-cols: 2; --md-cols: 3; --lg-cols: 4; --gap: 1rem;"
>
  <div>...</div>
  <div>...</div>
  <div>...</div>
  <div>...</div>
</div>

<!-- A responsive grid using auto-fill -->
<div
  class="layout-grid"
  style="--cols: auto-fill; --cols-size: minmax(min(100%, 15rem), 1fr); --gap: 1rem;"
>
  <div>...</div>
  <div>...</div>
  <div>...</div>
  <div>...</div>
</div>
```

### Pin

Control how an element is positioned in the DOM and its placement, as well as which z-index layer to use.

```scss
// Default settings
.layout-pin {
  --position: static;
  --top: auto;
  --right: auto;
  --bottom: auto;
  --left: auto;
  --z-index: 1;
}
```

```html
<!-- Make an element sticky -->
<div class="layout-pin" style="--position: sticky; --top: 0; --z-index: 200;">
  <div>...</div>
  <div>...</div>
  <div>...</div>
</div>

<!-- Make an element full size relative to its closest parent with relative position -->
<div class="layout-pin" style="--position: relative;">
  <div class="layout-pin" style="--position: absolute; --top: 0; --right: 0; --bottom: 0; --left: 0;">...</div>
  <div>...</div>
  <div>...</div>
</div>
```

### Switch

Switch lets the container space control the behavior of the content by switching from a single column to a multi column layout when the width of the parent element is equal the breakpoint.

#### Class

`.layout-switch`

Variable | Default | Options | Breakpoints | Description
---|---|---|---|---
`--breakpoint` | `0` | custom (it can be any unit except percentage) | `false` | Set the breakpoint value for when to trigger the layout switch.
`--cols` | `1` | integer | `false` | Set the number of columns to use when switching to a multi column layout.
`--gap` | `0rem` | custom (needs a unit so the calculation for the `min-width` property does not break) | `false` | Set the `gap` property. The value is also used in a calculation for the `min-width` property used on each column.
`--col-gap` | `undefined` | custom (needs a unit, if defined, so the calculation for the `min-width` property does not break) | `false` | Set the `--col-gap` if you need different values for `column-gap` and `row-gap`. `--col-gap` must be the same as the `column-gap` set in the `gap` shorthand property.

`.layout-switch > *`

Variable | Default | Options | Breakpoints | Description
---|---|---|---|---
`--col` | `var(--cols)` | custom | `false` | Overwrites `--cols` if, for example, you want different sized columns.
`--grow` | `0` | integer | `false` | Set the `flex-grow` poperty.

#### Usage

```html
<!-- Switch to a multi column layout when the parent width is equal to 40rem -->
<div class="layout-switch" style="--cols: 2; --breakpoint: 40rem;">
  <div>...</div>
  <div>...</div>
</div>

<!-- Switch to a multi column layout with different sized columns using a twelve column base -->
<div class="layout-switch" style="--breakpoint: 40rem;">
  <div style="--col: calc(12/5);">...</div>
  <div style="--col: calc(12/7);">...</div>
</div>
```
### Wrap

Wrap creates a responsive wrapping layout that group, pad and align content with the ability to customize it as needed.

#### Class

`.layout-wrap`

Variable | Default | Options | Breakpoints | Description
---|---|---|---|---
`--width` | `calc(100% - var(--padding))` | custom | `false` | Set the `width` property. A calc function is used by default to remove the padding from the the width through the `--padding` variable.
`--max-width` | `80rem` | custom | `false` | Set the `max-width` property.
`--margin-right` | `auto` | custom | `false` | Set the `object-fit` property.
`--margin-left` | `auto` | custom | `false` | Set the `spect-ratio` property.
`--padding` | `clamp(2rem, 10vw, 4rem)` | custom | `false` | Set the padding value to be used in the calc function for the `--width` variable. A clamp function is used by default.

#### Usage

```html
<!-- Wrap page content with default settings -->
<div class="layout-wrap">
  <header>...</header>
  <main>...</min>
  <footer>...</footer>
</div>

<!-- Wrap an article to a more readable length -->
<div class="layout-wrap" style="--max-width: 40rem;">
  <article>...</article>
</div>

<!-- Wrap an element without padding -->
<div class="layout-wrap" style="--padding: 0;">
  <div>...</div>
</div>
```

## Design Tokens

Will be updated soon.

## Functions

Will be updated soon.
