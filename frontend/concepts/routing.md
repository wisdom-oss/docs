# Routing

In this document, we'll discuss how routing works in WISdoM and how you can add 
and customize routes in your application.

## Routing Basics

Routing in WISdoM is based on the Angular Router class and follows the Angular 
way of defining routes. 
The `ModuleProvider` inserts the routes as children of the "" route. 
To enable the provider to find these routes, a valid 
[`Route`](https://angular.io/api/router/Route) object needs to be in the
`wisdomInterface` of the module.

All routes are secured by default.

## Adding Routes

To add routes to your application, you can define them in the `routes` property 
of a module's `wisdomInterface`. 
Here's an example of how to add a route:

```ts
import { Route } from '@angular/router';
import { MyComponent } from './my-component/my-component.component';

export const myRoute: Route = {
  path: 'my-component',
  component: MyComponent,
};
```

Then, in the `wisdomInterface`, you can add the `myRoute` object as a property:

```ts
import {WisdomInterface} from "common";
import {myRoute} from "./lib/routing";

export const wisdomInterface: WisdomInterface = {
  route: myRouter,
  ...
}
```

This will insert the routes and make them available to navigate to. 
However, to make them easily accessible to the user, they need to be part of the 
sidebar.

## Customizing Routes

Routes can be customized in a number of ways, including adding guards, 
resolvers, and other features. 
You can read more about customizing routes in the 
[Angular documentation](https://angular.io/guide/routing-overview).

## Sidebar

The sidebar is a central part of WISdoM's UI and allows users to easily 
navigate to different parts of the application. 
To add a module to the sidebar, you need to create a link to it in the 
`wisdom.config.ts` file. 
You can read more about this in the 
[Building and Customizing the Frontend Application](../build-and-customize/building-and-customizing-the-frontend-application.md) 
document.
