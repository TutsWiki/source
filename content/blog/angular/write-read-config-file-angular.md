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

Ever wondered where to store configurations in your code? The most common case is to store environment configurations which are required when you have multiple environments - local, dev, qa, uat and prod. Well this article will tell you how to write and read config files in Angular. There is a defined place for storing them and you should make most use of it. But before going to that let's check out the wrong places to store the configs.

## Environment.ts

Wrong!

I know this comes as first option in our mind but can you see "config" in it's name? No, right? Cause it was never meant to store configuration in this file. The sole purpose of this is to tell the run time what version of the code you want to run.

Now you may argue that we can create different environment.ts files for different versions, yes you can but again this is not the purpose of this file.

Then where to store your config?

One thing is certain - store them outside of your code. But where? One option can be to have a static json file in assets or dist and use it to get and set the configurations. But this sounds hacky.

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

So let's move to our actual solution which is:

## APP_INITIALIZER

Sounds awesome right? It will specify a `factory` and that will return a `promise`, the `promise` will load the config for the application. After loading the configs you can resolve the `promise`. It looks like this:

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

In this code sample we have added `APP_INITIALIZER` to `app.module.ts`. It will call the initialize method. You can use this method to load configurations and initialize your app. As told above, this will return a `promise`. You can replace `resolve(true)` with your code or can write a new `promise`.

One more thing:

### How to add dependencies in this method?

To add dependencies, add a `deps` section in its provider. Example:

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

and add them as parameter to our initialize method.

```js
function initialize(http: HttpClient, userInfoService: UserInfoService, config: ConfigService) {
    return (): Promise<boolean> => {
    return new Promise<boolean>((resolve: (a: boolean) => void): void => {
      resolve(true);
    });
};
```

Hope this will guide you on how to read and write config in Angular.