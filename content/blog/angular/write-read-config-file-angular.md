---
date: 2020-08-12
linktitle: Writing and Reading config files in Angular
title: Writing and Reading config files in Angular
weight: 10
url: /read-write-config-files-in-angular
description: Learn how to write and read config files in Angular using configparser module.
keywords:
  - angular
  - config
  - read config file
  - write config file
  - APP_INITIALIZER
tags: [angular]
---
<meta property="og:image" content="https://tutswiki.com/img/tutswiki.png"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="Writing and Reading config files in Angular" />
<meta name=”twitter:description” content="Learn how to write and read config files in Angular using APP_INITIALIZER." />

Every app has some configurations to load. The most common is environment configurations which are required when you have multiple environments - local, dev, qa, uat and prod. Well this article will tell you where to store configurations and how to read them in Angular. 

## Storing config in Angular

Let's say we need to store the baseUrl of our app. Where do you think this should be added? There is a defined place for storing config in Angular and you should make most use of it. But before going to that let's check out the wrong place to store the configs.

### Environment.ts

**Wrong!**

I know this comes as first option in our mind but can you see "config" in it's name? No, right? Cause it was never meant to store configurations in this file. The sole purpose of this is to tell the run time what version of the code you want to run.

Now you may argue that we can create different environment.ts files for different versions, yes you can but again this is not the purpose of this file.

Then where to store your config?

One thing is certain that we need a static json file to store our configs so let's first create a config file and name it `config.json`. It will look something like this:

```js
{
    "baseUrl": "https://tutswiki.com/"
}
```

Now we have `config.json` but we will need a place for it. One option is to have it in `assets` or `dist` and use it to get and set the configurations. But is this recommended? **No!**

So let's move to our actual solution which is:

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-9878675755379402"
     data-ad-slot="5842766387"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## APP_INITIALIZER

Sounds awesome right? It will specify a `factory` and that will return a `promise`, the `promise` will load the config for the application. After loading the configs you can resolve the `promise`.

Let's take an example.

```js
import { APP_INITIALIZER } from '@angular/core';

function initialize() {
	return (): Promise<boolean> => {
    return new Promise<boolean>((resolve: (a: boolean) => void): void => {
      resolve(true);
    });
  };
  
@NgModule({
  declarations: [
  ],
  imports: [    
  ],
  providers: [{
    provide: APP_INITIALIZER,
    useFactory: initialize,
    multi: true
  }],
  bootstrap: []
})
export class AppModule { }
```
`useFactory` will have the function which will return a function which will return a promise.

`multi: true` will allow to have multiple instances of the provider. They are all able to run simultaneously but the code will be stopped here until we get all the promises resolved.

In this code sample we have added `APP_INITIALIZER` to `app.module.ts`. It will call the `initialize` method. You can use this method to load configurations and initialize your app. As told above, this will return a promise. You can replace `resolve(true)` with your code or can write a new `promise`.

### How to add dependencies in this method?

To add dependencies, add a `deps` section in its `provider`. Example:

```js
providers: [{
  provide: APP_INITIALIZER,
  useFactory: initialize,
  deps: [
      HttpClient,
	  UserInfoService,
      ConfigService
    ],
  multi: true
}],
```
  
and add them as parameter to our `initialize` method.

```js
function initialize(http: HttpClient, userInfoService: UserInfoService, config: ConfigService) {
	return (): Promise<boolean> => {
    return new Promise<boolean>((resolve: (a: boolean) => void): void => {
      resolve(true);
    });
};
```

## Reading config file in Angular

First we will need a service to save the configs we are getting from `config.json` so let's create a `ConfigService`.

```js
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class ConfigService {
  baseUrl: string;
  constructor() { }
}
```

We have already added this in `deps` of `APP_INITIALIZER`.

Now let's use `initialize` method to read our config file and set the necessary details for our app. 

```js
function initialize(http: HttpClient, config: ConfigService): (() => Promise<boolean>) {
  return (): Promise<boolean> => {
    return new Promise<boolean>((resolve: (a: boolean) => void): void => {
       http.get('./config.json')
         .pipe(
           map((x: ConfigService) => {
             config.baseUrl = x.baseUrl;
             resolve(true);
           })
         ).subscribe();
    });
  };
}
```

And that's how we deal with config in Angular.
