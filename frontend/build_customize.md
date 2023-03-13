# Building and Customizing the Frontend Application

This document outlines the steps required to build the frontend application and 
customize it to fit your needs.

## Building the Application

Although building the application is designed to be kept simple, Angular is not 
really made for a modular design, which makes the build process slightly more 
challenging than usual. 
To build the application, some important dependencies should be globally 
installed:

- **Node.js**:
  This is a JavaScript runtime environment required to run the Angular CLI and 
  build the application.

- **pnpm**:
  This is the package manager that allows reusing dependencies. 
  While not entirely necessary for this project as all dependencies are in the 
  workspace, other WISdoM Node.js applications also use it.

- **meta**: 
  This is a repository manager that allows cloning and commanding multiple 
  repositories at once.

- **Angular CLI**: 
  This is required to build the application itself. 
  Once the workspace is installed, the local installation may overtake it.

### Install Node.js
Install yourself Node.js from their 
[official website](https://nodejs.org/en/download/) and install it.
You may use a package manager of your choice, just make sure you have current 
version or the LTS listed on the Node.js website.

You can check your Node version by running:

```sh
node -v
```

### Install Global CLI Tools

After Node.js you also need `pnpm`, `meta` and `@angular/cli`.
You can simply install them by running:

```sh
npm install -g pnpm meta @angular/cli
```

### Clone the Workspace

Clone the workspace somewhere on your local machine:

```sh
git clone git@github.com:wisdom-oss/frontend-workspace.git
cd frontend-workspace
```

*Note: The link could also be forked repository.*

### Update your Workspace with Meta

After cloning the repository you need to clone the subrepositories, this is done 
via meta:

```sh
meta git update
```

### Install Dependencies

To install the dependencies, simply run:

```sh 
pnpm install
```

### Build your Workspace

You can simply build your workspace via the included `build.js` file, simply run:

```sh
node build.js
```

### Serve the Workspace

After that you can serve your workspace, run:

```sh
ng serve
```

Now your workspace should be available at 
[`https://localhost:4200`](https://localhost:4200).

## Customizing the Application

To customize the application to fit your needs, it is recommended that you clone 
the `workspace`, `core`, and `common` repositories. 
The `workspace` repository is where you can configure the entire workspace, 
while the `core` repository contains the core of the Angular application, 
including the frame layout, authentication, networking, routing, and i18n. 
The `common` repository contains a library of mostly utility components, 
services, and other elements that are heavily customizable and reusable 
throughout the project.

Keep in mind that the `core` and `common` repositories are meant to be used as 
is, and any modifications should be made with care, as they may affect other 
parts of the application.
They also may be updated to accommodate for more core features, all of them will 
always be configurable via the config files, which should allow to keep these 
modules untouched by externals.

Once you have cloned the repositories, you can customize the application as 
needed. 
There are two main files that you can modify to configure your application:

- [`wisdom.config.ts`](github.com/wisdom-oss/frontend-workspace/blob/main/wisdom.config.ts): 
  This TypeScript file is used to configure the entire workspace, including the 
  sidebar, authentication, and other features. 
  It can be modified to fit your specific needs and preferences.

- [`wisdom.modules.ts`](https://github.com/wisdom-oss/frontend-workspace/blob/main/wisdom.modules.ts): 
  This TypeScript file is a hack to make the `core` work with the modules with 
  as little effort as possible. 
  It is used to re-export the modules installed from `@WISdoM-OSS` or add your 
  own modules, which are then imported by the 
  [`ModuleProvider`](https://github.com/wisdom-oss/frontend-core/blob/main/src/app/module-provider.ts) 
  to expose them to the routes of the `core`.
