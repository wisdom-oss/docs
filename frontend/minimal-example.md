# Minimal Example

The minimal example of a working module can be found 
[here](https://github.com/wisdom-oss/frontend-example).
It is a simple starting point to play with a working module that is already 
correctly configured.
The example includes one module, one component and one service.

## Prerequisites

To run and build the minimal example you should have a working instance of the 
`workspace`, check [this](building-and-customizing-the-frontend-application.md) on how to achieve this.

## Getting Started

### Clone
To get started, clone the `frontend-example` using the `.meta` file in your 
workspace:

```json 
{
  "projects": {
    "./wisdom_modules/core": "https://github.com/wisdom-oss/frontend-core.git",
    "./wisdom_modules/common": "https://github.com/wisdom-oss/frontend-common.git",
    "./wisdom_modules/example": "https://github.com/wisdom-oss/frontend-example.git"
  }
}
```

```sh
meta git update
```

### Install

Then install the application:

```sh
pnpm install
```

### Configure

The example also needs to be configured in the workspace.
The `angular.json` needs a new entry in `projects`:

<details>

```json
"example": {
  "projectType": "library",
  "root": "wisdom_modules/example",
  "sourceRoot": "wisdom_modules/example/src",
  "prefix": "lib",
  "architect": {
    "build": {
      "builder": "@angular-devkit/build-angular:ng-packagr",
      "options": {
        "project": "wisdom_modules/example/ng-package.json"
      },
      "configurations": {
        "production": {
          "tsConfig": "wisdom_modules/example/tsconfig.lib.prod.json"
        },
        "development": {
          "tsConfig": "wisdom_modules/example/tsconfig.lib.json"
        }
      },
      "defaultConfiguration": "production"
    },
    "test": {
      "builder": "@angular-devkit/build-angular:karma",
      "options": {
        "main": "wisdom_modules/example/src/test.ts",
        "tsConfig": "wisdom_modules/example/tsconfig.spec.json",
        "karmaConfig": "wisdom_modules/example/karma.conf.js"
      }
    }
  }
}
```

</details>

The `wisdom.config.ts` expects a route for the new module:

```ts
export const sidebar: SideBarEntries = [
  [
    "Proof of Concept", "rocket", [
      ["Example", "sparkles", "example"]
    ]
  ]
];
```

The `wisdom.modules.ts` expects a re-export of the module:

```ts
export * as example from "example";
```

## Run

To run the example in a dev environment you need to build every component.
An easy way would be the `build.js` file.

After that you can serve the files locally with:

```sh
ng serve
```

This will make the application available at ```https://localhost:4200```, you 
probably need to bypass the browser security warning or add your own 
certificate.
