# Icons

This document outlines how to use icons in WISdoM and provides guidance on the 
icon set used by the application.

## Icon Set

WISdoM uses the [IonIcons](https://ionic.io/ionicons/) icon set.
IonIcons is a relatively small icon set that provides a coherent set of modern 
icons. 
It is the only icon pack used in WISdoM to achieve a consistent and modern 
styling.

## Usage

The `common` library contains a component called `IonIconComponent`, which has 
the selector `ion-icon`. 
It allows the easy use of IonIcons in a template. 
Here is an example of how to use the `IonIconComponent` in an HTML template 
using [Bulma](styling-with-bulma.md):

```html
<p class="icon-text">
  <span class="icon">
    <ion-icon name="rocket"></ion-icon>
  </span>
  <span>Cool feature</span>
</p>
```

The `name` attribute of the `ion-icon` element specifies the name of the 
IonIcon to use.

In many places of the application can IonIcons be used directly by their name.
For example, to use the IonIcon `rocket` in the sidebar configuration, you can 
simply specify the name as follows:

```ts
export const sidebar: SideBarEntries = [
  [
    "My category", "rocket", [
    ["Example", "sparkles", "example"]
  ]
  ]
];
```

## Considerations

It is recommended to only use IonIcons in WISdoM to maintain a consistent and 
modern styling throughout the application. 
Using another icon set could potentially disrupt the overall design.
