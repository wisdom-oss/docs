# Creating Your Own Module

This tutorial will guide you through creating your own module and integrating it 
into the WISdoM frontend.

## Prerequisites

Before creating your own module, make sure that you have a working instance of 
the `workspace`. 
If you haven't set it up yet, you can follow the steps outlined in the 
[build and customize](building-and-customizing-the-frontend-application.md) 
guide.

## Getting Started

### Generate a New Library

First, generate a new library using the Angular CLI by running the following 
command:

```sh
ng generate library my-lib
```

This will create a new library named `my-lib` in the `wisdom_modules` directory. 
The `my-lib` directory will contain a starter `public-api.ts` file and a `lib` 
directory containing a sample component.

### Define WISdoM Interface

To make your library work with the WISdoM frontend, you need to define a 
`WisdomInterface` called `wisdomInterface` in your `public-api.ts` file. 

> ⚠️ **Caution**: It is very important that the exported constant is called 
> `wisdomInterface` as `core` module assumes specifically that name

This interface should include the following properties:

```ts
import { WisdomInterface } from 'common';
import { MyLibComponent } from './lib/my-lib.component';

export const wisdomInterface: WisdomInterface = {
  route: {
    path: 'my-lib',
    component: MyLibComponent,
  },
  scopes: [],
  translations: {
    de_DE: {},
    en_US: {},
  },
};
```

- `route`:
  A route that points to your component. 
  This is necessary to make your component accessible as an entry point in the 
  WISdoM frontend. 
  The `path` property specifies the URL path to your component and the 
  `component` property specifies the component to be displayed when the route is 
  activated.

- `scopes`:
  An array of scopes that a user must have to access your component.
  *Note: This may change in the near future.*

- `translations`:
  An object containing translations for your component. 
  This is used for i18n and can be empty if your component does not require 
  translations.

### Export Your Module

Next, you need to export your module in the `wisdom.modules.ts` file so that it 
can be accessed by the `core` module. 
Add the following line to the file:

```ts
export * as myLib from 'my-lib';
```

Make sure to replace `myLib` with a constant name of your choice. 
This constant will be used later for routing.

### Integrate Your Module in the Sidebar

To make your module accessible to users, you need to add an entry for it in the 
sidebar. 
Open the `wisdom.config.ts` file and add an entry for your module to the 
`sidebar` constant, for example:

```ts
["My Category", "browsers", ["My Component", "balloon", "myLib"]]
```

This would add a category called "My Category" with the "browsers" 
[ionicon](https://ionic.io/ionicons/) and an entry called "My Component" with 
the "balloon" icon, directing to your component in your library.
The display names here may be translated, see [this](broken link).

### Build Your Module

To build your module, run the following command:

```sh 
ng build my-lib --watch
```

This command will build your module and watch for changes, allowing for hot 
reloading.

You also need to build the `core` module to see the changes in the frontend. 
Run the following command in a separate terminal:

```sh 
ng build core --watch
```

### Add an Entry to `.meta`

To make it easy for others to install your module, you should add an entry for 
it to the `.meta` file under `projects`:

```json
"./wisdom_modules/my-lib": "git@your-git.org:your-org/my-lib.git"
```
