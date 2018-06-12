---
date: 2018-06-11
linktitle: Angular 6 features
title: What's new in Angular 6 (Features List)
weight: 10
url: /angular6-features
description: List of major changes in Angular 6. ng-add, ng-update, material, cdk and starter components.
keywords:
  - angular 6
  - angular 6 features
  - angular 6 release date
  - angular cli
  - angular cdk
  - bootstrap
  - ng update
  - ng add
  - rxjs
  - material
  - release notes
  - components
---
<meta property="og:image" content="https://tutswiki.com/images/Angular6.png"/>
[Angular 6](https://angular.io/) is out with all new features. It is a major release in which Angular team has synchronized many of the important framework packages to make most out of cross compatibility. All the major framework packages like [@angular/core](https://angular.io/api/core), [@angular/compiler](https://angular.io/api/core/Compiler), [@angular/common](https://angular.io/api/common) etc., are reworked and released as version 6.0.0. Below is the list of major changes in Angular 6, let's explore them one by one.

![Angular version 6](/images/Angular6.png "Angular 6.0.0")

## Angular 6 Features:
 - New CLI commands
    - ng update
    - ng add
 - Referencing providers
 - Angular Elements
 - Angular Material + CDK Components
    - Tree
    - Badge
    - Bottom-Sheet
    - Overlay
 - Starter Components
    - Dashboard
    - Side-Nav
    - Datatable
 - Library Support
 - CLI Workspaces
 - RxJS v6
 - Long Term Support

### ng update

A new command is introduced in Angular CLI which analyzes package.json and recommends any updates (if needed) on the installed packages.

This is going to make your life easy as now you don't have to manually synchronize your dependencies. This will work using [Schematics](https://blog.angular.io/schematics-an-introduction-dc1dfbc2a2b2), a workflow tool which is based on Tree data structure. For example a tree has base, scripts following schematics will also have a base and that base will be the root structure. Any modification has first have to pass from staging area to make its way to the root. If any of the dependency provide a schematic then it can automatically update your code whenever there are major changes.

So now if you run ```ng update @angular/common``` it will update the package and run any schematic available for this package. 

![ng-update in angular 6](/images/ng-update.png "ng-update")

To get more details on ng update usage, refer the [official specification](https://github.com/angular/angular-cli/blob/master/docs/specifications/update.md).

### ng add

This is another CLI command added in Angular 6. This will be helpful in adding new packages to your project. It downloads the packages (with their dependencies) using the package manager and installs them with the help of Schematics.

```javascript
ng add @angular/material // Install and configure Angular Material
ng add @ng-bootstrap/schematics // Add ng-bootstrap in your application
```

![ng-add in angular 6](/images/ng-add.png "ng-add")

This area will flourish with increase in packages supporting ```ng add```. Refer [Material's ng-add schematic](https://github.com/angular/material2/blob/master/src/lib/schematics/collection.json) to get an idea on how to write one for your application.

### Referencing Providers

Before this update we used to reference services from module but with Angular 6 we will reference module from service.

![Referencing providers in angular 6](/images/referencing-providers.png "Referencing providers")
![Tree Shakable Providers in angular 6](/images/Tree-Shakable-Providers.png "Tree Shakable Providers")

### Angular Elements

[@angular/elements](https://angular.io/guide/elements) creates a custom element which acts as a bridge between Angular component interface and change detection functionality to the built-in DOM API. You can bootstrap Angular components with your application and they will be registered as Angular Elements. One of the benefits that you get from this is that it'll dynamically insert HTML code holding Angular components after your application is compiled.

### Angular Material + CDK Components

Below are some new components that have been added in Angular 6.

  - **Tree:** It is based on datatable component and is best suited for representing hierarchical data. CDK contains the core tree directive (cdk-tree) whereas mat-tree comes with Material design style. See [this presentation](https://docs.google.com/presentation/d/1DmWdfr8j25owK2ac5qlt7oeX6HpxQnXEGwmHIjf6EHI/edit#slide=id.g26d86d3325_0_0) from [ng conf 2018](https://www.ng-conf.org/sessions/) to get more details on Material Trees.

![cdk mat tree in Angular 6](/images/cdktree-matree-Angular.png "Nested Tree")

  - **Badge:** This component can be used to display a small piece of information such as notification count on notification drawer, unread emails count over the email icon etc.

![Badges in Angular 6](/images/Badges-Angular-6.png "Badges")

  - **Bottom-Sheet:** This component is mobile-centric which can be used to present a list of options for a particular action. It appears from the bottom of the viewport (with slide-up animation). 

![Bottom Sheet Angular 6](/images/Bottom-Sheet-Angular-6.png "Bottom-Sheet")

  - **Overlay:** ```@angular/cdk/overlay``` has been updated with a new positioning logic which helps in creating intelligent pop-ups that remain on the screen in all situations.
  
### Starter Components

With Angular 6 you get 3 new starter components after running ```ng add @angular/material```. With this update in place, it's very easy to create simple UI.

  - Create a dashboard

```javascript
ng generate @angular/material:material-dashboard --name=test-dashboard
```
![dashboard in Angular 6](/images/dashboard.png "Material Dashboard")

  - Create side navigation

```javascript
ng generate @angular/material:material-nav --name=test-nav
```
![side navigation in Angular 6](/images/side-navigation.png "side navigation")

  - Create a datatable

```javascript
ng generate @angular/material:material-table --name=test-table
```
![material datatable in Angular 6](/images/datatable.png "datatable")

### Library Support

Library support has been added in 6.0.0. You can easily create a library using below command. [See details](https://github.com/angular/angular-cli/wiki/stories-create-library).

```javascript
ng generate library <name>
```

### CLI Workspaces

CLI can now support workspaces with multiple projects. ```angular-cli.json``` has been renamed to ```angular.json```. Each workspace can have multiple projects, each project can have targets and each target can have a configuration.

![CLI workspaces in Angular 6](/images/cli-workspaces.png "CLI workspaces")

### RxJS v6

Angular 6 supports [RxJS v6](https://rxjs-dev.firebaseapp.com/). RxJS v6 ensures that only those modules are bundled in production that are being used by your application. If you are moving from the previous version to Angular 6 then you have to change import statements and operator usage to make your application work. But if you don't want to change then you can use this new package ```rxjs-compat``` which provides backward compatibility.

### Long Term Support

Angular team announced to provide long-term support for all major releases starting from version 4. LTS will include 6 months of active development + 12 months of security patches and critical bug fixes.

### Other Changes:

 - ```<template>``` tag has been removed so now you have to use ```<ng-template>``` instead.
 - ```@angular/http``` is deprecated, ```@angular/common/http``` is the recommended alternative.

{{% notice note %}}
If above-listed features look promising to you and you want to use Angular 6 for your existing project, then follow https://update.angular.io/.
{{% /notice %}}
