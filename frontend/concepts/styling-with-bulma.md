# Styling with Bulma

This document is about how the WISdoM application is styled.

The entire application uses the [Bulma CSS framework](https://bulma.io) and some 
of [its extensions](https://bulma.io/extensions/). 
Bulma is an open-source CSS-only framework that makes the application 
lightweight and fast. 
Almost all design components only have one version, but with different 
additional configuration settings.

## Benefits of using Bulma

One of the advantages of using Bulma is that it provides consistent styling and 
coloring by only using a handful of color classes. 
Dark mode is added via the 
[`bulma-prefers-dark`](https://www.npmjs.com/package/bulma-prefers-dark) package 
and is automatically applied when the user demands a dark mode via the 
browser/OS options. 
It should be avoided to use another CSS framework anywhere in the project.

## Using Bulma

All Bulma elements can be created using a handful of standard HTML elements and 
CSS classes with clear names. 
The Bulma classes are globally available, making the CSS file for a component 
often obsolete.

## Developing Smart Features

Since Bulma has no JavaScript, all smart features need to be developed in this 
application. 
Some of these features can be found in the `common` package under `bulma`.

## Example HTML Template Using Bulma

Here's an example HTML template using Bulma:

```html
<!-- Bulma is globally available. -->
<div class="card">
  <div class="card-header">
    <div class="card-header-title">Example here</div>
  </div>
  <div class="card-image">
    <figure class="image is-4by3">
      <img src="https://bulma.io/images/placeholders/1280x960.png"/>
    </figure>
  </div>
  <div class="card-content">
    <div class="content">
      <p>Example Content</p>
    </div>
  </div>
</div>
```

This template demonstrates how Bulma can be used to create a simple card with a 
header, image, and content. 
The `card` class is one of the many classes provided by Bulma. 
Bulma also provides classes for colors, sizes, and spacing, among other things.

## Additional Bulma Helpers

In addition to the standard helpers provided by
[Bulma](https://bulma.io/documentation/helpers/)
for defining Color, Spacing, Typography, Visibility, Flexbox, and more,
WISdoM incorporates a few custom helpers to enhance functionality.
Since Bulma is a CSS-only library, some helpers necessitate the implementation 
through JavaScript and are thus not inherently part of Bulma.

#### .is-toggleable

The `.is-toggleable` helper is provided by the `common` package via the 
`BulmaIsToggleableDirective`. 
This helper allows elements to toggle the visibility class, making it especially 
handy for interactive components like dropdowns.

##### Example

```html
<div class="dropdown is-toggleable">
  <div class="dropdown-trigger">
    <button class="button">
      <span>Toggle</span>
    </button>  
  </div>
  <div class="dropdown-menu">
    <div class="dropdown-content">
      <a href="#" class="dropdown-item">Item</a>
    </div>
  </div>
</div>
```

In this simplified example, clicking the button will toggle the visibility of 
the dropdown menu through the application of the `.is-toggleable` helper.

### .is-auto-hide

The `.is-auto-hide` helper is provided by the `core`.
It facilitates the automatic removal of the visibility class from elements when 
a click is detected outside the element, effectively hiding the element.
This helper can be located in the `AppComponent` of the `core`.

#### Example

```html
<div class="dropdown is-auto-hide">
  <div class="dropdown-trigger">
    <button class="button">
      <span>Auto Hide</span>
    </button>
  </div>
  <div class="dropdown-menu">
    <div class="dropdown-content">
      <a href="#" class="dropdown-item">Item</a>
    </div>
  </div>
</div>
```

In this instance, the dropdown menu will automatically hide when a click is 
detected outside of it, thanks to the `.is-auto-hide` helper.
