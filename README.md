# vue-fraction-grid

[![Build Status](https://travis-ci.org/bkzl/vue-fraction-grid.svg?branch=master)](https://travis-ci.org/bkzl/vue-fraction-grid)

Flexbox based responsive fraction grid system for Vue.js

### [Live Demo and Full Documentation](https://bkzl.github.io/vue-fraction-grid)

```html
<container>
  <grid vertical="middle" :rwd="{compact: 'stack'}">
    <grid-item size="1/6" :rwd="{compact: '0/1'}">
      ...
    </grid-item>

    <grid-item size="3/6" :rwd="{compact: '1/1'}">
      ...
    </grid-item>

    <grid-item size="2/6" :rwd="{compact: '1/1'}">
      ...
    </grid-item>
  </grid>
</container>
```

## Installation

1. Install package using `yarn` or `npm`:

    ```sh
    $ yarn add vue-fraction-grid

    # or

    $ npm install --save vue-fraction-grid
    ```

2. Load the plugin by calling `Vue.use()`:

    ```js
    import Vue from 'vue'
    import VueFractionGrid from 'vue-fraction-grid'

    Vue.use(VueFractionGrid)
    ```

3. Now you have access to three global components in your templates: `<container>`, `<grid>`, `<grid-item>`

## Settings

```js
Vue.use(VueFractionGrid, {
  container: '1020px',
  gutter: '24px',
  approach: 'mobile-first',
  breakpoints: {
    compact: '320px 414px'
  }
})
```

Configure grid by passing options as a second argument of `Vue.use()`. Configuration is merged to defaults that are listed above.

Available settings:

```
container   - Default containers width. Works with any valid CSS values like: '1440px',
              '52em', '100vw' etc. Set it to '100%' to use full-width fluid grid. Because
              this is the maximum value, the grid will scale nicely for smaller viewports.
gutter      - Gutter width, works with any valid CSS values like '30px', '1rem' etc.
approach    - 'mobil-first' or 'desktop-first'. Choose which approach of responsive web design
              do you prefer. Breakpoint rules are based on this option.
breakpoints - List the grid breakpoints. Key is the breakpoint name for `:rwd` prop.
              Value is the size and can include one or two CSS values separated
              with a space.
```

Detailed explanation and more examples are in [the full documentation](https://bkzl.github.io/vue-fraction-grid).

## API

### Container

Syntax:

```html
<container [width="<length>|<percentage>"]>
```

Center content of the site in the container:

```html
<container>
  ...
</container>
```

Override container's maximum width with `width` prop. This is useful when you need more than one container's type e.g. regular and full-width.

```html
<container width="100%">
  ...
</container>
```

### Grid

Syntax:

```html
<grid [horizontal="left|center|right"
      vertical="top|middle|bottom"
      direction="reverse|stack|stack-reverse"
      :rwd="{ breakpointName: direction }"
      flat
      pair]>
```

The most common used and simple example of the grid:

```html
<grid>
  <grid-item size="1/3">
    ...
  </grid-item>

  <grid-item size="2/3">
    ...
  </grid-item>
</grid>
```

Nest grids however you want to, the gutter is always the same. There is no need to wrap nested grids with containers.

```html
<grid>
  <grid-item size="2/3">
    <grid>
      <grid-item size="1/2">
        ...
      </grid-item>

      <grid-item size="1/2">
        ...
      </grid-item>
    </grid>
  </grid-item>

  <grid-item size="1/3">
    ...
  </grid-item>
</grid>
```

Horizontal alignment of grid items. This works simillar to `justify-content` attribute.

```html
<grid horizontal="left">
  ...
</grid>
```

```html
<grid horizontal="center">
  ...
</grid>
```

```html
<grid horizontal="right">
  ...
</grid>
```

```html
<grid horizontal="around">
  ...
</grid>
```

```html
<grid horizontal="between">
  ...
</grid>
```

Vertical alignment of grid items. This works simillar to `align-items` attribute.

```html
<grid vertical="top">
  ...
</grid>
```

```html
<grid vertical="middle">
  ...
</grid>
```

```html
<grid vertical="bottom">
  ...
</grid>
```

Remove gutter from grid items.

```html
<grid flat>
  ...
</grid>
```

Align content of the first item to the left and content of the second item to the right.

```html
<grid pair>
  ...
</grid>
```

Set the grid items direction. This works simillar to `flex-direction` attribute.

```html
<grid direction="reverse">
  ...
</grid>
```

```html
<grid direction="stack">
  ...
</grid>
```

```html
<grid direction="stack-reverse">
  ...
</grid>
```

### Grid Item

Syntax:

```html
<grid-item size="<number>/<number>"|grow="<number>"|shrink="<number>"
           [:rwd="{ breakpointName: size }"]>
```

Use any size in the fraction format. Grid item should be nested directly in the grid. Fractions of items don't have to sum to 1. **Denominator can't be equal 0!**

```html
<grid-item size="5/8">
  ...
</grid-item>
```

```html
<grid-item size="1/10">
  ...
</grid-item>
```

Fill the grid with a grid item by setting its size to `1/1`:

```html
<grid-item size="1/1">
  ...
</grid-item>
```

Hide the grid item by setting its size to `0/1`:

```html
<grid-item size="0/1">
  ...
</grid-item>
```

Use `grow` and `shrink` props instead of `size`. They work simillar to `flex-grow` and `flex-shrink` attributes. **Never mix size, grow and shrink into a single item!**

```html
<grid-item grow="2">
  ...
</grid-item>
```

```html
<grid-item shrink="0.5">
  ...
</grid-item>
```

### Responsive Design

Responsive design is based on two options from settings: `approach` and `breakpoints`.

If you set approach to `mobile-first` breakpoints with a single value will respond to media queries using `min-width` attribute. If you use `desktop-first` instead, breakpoints will respond to `max-width` attribute.

Breakpoints with two values respond to `(min-width: <length>) and (max-width: <length>)` query.

Following configuration:

```js
Vue.use(VueFractionGrid, {
  approach: 'desktop-first',
  breakpoints: {
    compact: '414px',
    tablet: '415px 1100px'
  }
})
```

Is translated to this declaration from CSS:

```css
@media (max-width: 414px) { ... } /* compact */
@media (min-width: 415px) and (max-width: 1100px) { ... } /* tablet */
```

See more examples [in docs](https://bkzl.github.io/vue-fraction-grid).

### Responsive Design API

Change the direction of a grid for specific breakpoints.

```html
<grid :rwd="{compact: 'reverse'}">
  ...
</grid>
```

```html
<grid direction="stack" :rwd="{compact: 'stack-reverse'}">
  ...
</grid>
```

Change size of a grid item for specific breakpoints.

```html
<grid-item size="3/4" :rwd="{compact: '0/1'}">
  ...
</grid-item>
```

## Development

1. Clone the repository:

    ```sh
    git clone git@github.com:bkzl/vue-fraction-grid.git
    ```

2. Run scripts from package.json using `npm run` / `yarn run` in the main directory:

    ```
    demo - Start demo page at localhost:4000 with all API examples from README
    test - Run tests using Jest
    lint - Lint JS and CSS code of components, utils and installation
    ```

* * *

Code is open sourced [on GitHub](https://github.com/bkzl/vue-fraction-grid). Up to date changelog is available under [the releases section](https://github.com/bkzl/vue-fraction-grid/releases).

I appreciate any comments, feedback, and information about potential issues. Have you experienced a bug or noticed a mistake in documentation? Please [add a new issue](https://github.com/bkzl/vue-fraction-grid/issues). Thanks!
