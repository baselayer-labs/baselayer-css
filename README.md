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

## Documentation

### Layouts

#### Flex

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

#### Frame

Frame makes its child element responsive with a set aspect ratio. Often used for media elements.

```scss
// Default settings
.layout-frame {
  --object-x: 50%;
  --object-y: 50%;
  --object-fit: cover;
  --ratio: 1/1;
}
```

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

#### Grid

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

#### Switch

Switch from a single column to a multi column layout when the width of the parent element is equal the breakpoint. When using different gap values for row and column, you need to define `--col-gap` to get the correct column width.

```scss
// Default settings
.layout-switch {
  --breakpoint: 0;
  --cols: 1;
  --gap: 0rem;
  --multiplier: 9999;
  --trim: 0.001%;
}

.layout-switch > * {
  --col: var(--cols);
  --grow: 0;
  --basis: calc((var(--breakpoint) - 100%) * var(--multiplier));
  --gap-width: calc(var(--col-gap, var(--gap)) / var(--col));
  --num-gaps: calc(var(--col) - 1);
  --min-width: calc(100% / var(--col) - (var(--num-gaps) * var(--gap-width)) - var(--trim));
}
```

```html
<!-- Switch to a multi column layout when the parent width is equal to 40rem -->
<div class="layout-switch" style="--cols: 3; --breakpoint: 40rem;">
  <div>...</div>
  <div>...</div>
  <div>...</div>
</div>
```
#### Wrap

Wrap and align content with padding.

```scss
// Default settings
.layout-wrap {
  --width: calc(100% - var(--padding));
  --max-width: 80rem;
  --margin-right: auto;
  --margin-left: auto;
  --padding: clamp(2rem, 10vw, 4rem);
}
```

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

### Design Tokens

Will be updated soon.

### Utilities

Will be updated soon.

## Roadmap

- Add Baselayer CSS to NPM
- Add config for Purge CSS
