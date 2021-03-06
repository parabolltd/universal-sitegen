# Universal Sitegen
> An Angular Universal static site generator

<!-- TOC -->

- [Universal Sitegen](#universal-sitegen)
  - [What is this?](#what-is-this)
  - [Getting started](#getting-started)
    - [Installation](#installation)
    - [Building your static site](#building-your-static-site)
      - [CLI](#cli)
      - [Programmatically](#programmatically)

<!-- /TOC -->

## What is this?
Ever wanted to make a static site from your Angular app? Now that we have finalized Angular Universal, you can! Universal Sitegen takes your angular app, builds it, then spits out static pages for each route. If you're building a website that you don't need a client side JS framework for, then this is pefect. Blogs, landing pages, sales pages, launch pages, company sites, you get the point.


## Getting started
### Installation
* `yarn add @angularclass/universal-sitegen`

### Building your static site
There are two ways to build your site:
* [CLI](#cli)
* [Programmatically](#programmatically)

No matter what approach you use, you need the following setup before you're ready to go.

First, make sure your AppModule is importing `BrowserModule.withServerTransition()`
and your routes. As it should.

```typescript
// app.module.ts

// .....imports

@NgModule({
  imports: [
    BrowserModule.withServerTransition({ appId: 'my-site' }),
    ROUTES
  ],
  // declarations
  // providers
  // bootstrap
})

export class AppModule {}
```

Next, create a ServerModule for Universal, and import your AppModule

```typescript
// server.module.ts
import { NgModule } from '@angular/core'
import { ServerModule } from '@angular/platform-server'
import { App } from './app.component'
import { AppModule } from './app.module'

@NgModule({
  imports: [ServerModule, AppModule],
  bootstrap: [App]
})

export class AppServerModule {}
```

There is a [Demo App](https://github.com/angularclass/universal-sitegen/tree/master/demo) that has the basics. Great place to start to getting your app ready.

#### CLI
Make sure you followed the steps above first. To build your site with the provided CLI (preffered), you only need to do a few things.


Because your app is never going to be ran in the browser and only node, you might have to adjust your webpack config. Look at the [demo webpack config](https://github.com/angularclass/universal-sitegen/tree/master/demo/webpack.config.js) for what it should look like.

Next, build your app. After you build it, you need to create a `universal.js` file with options on the root of your app.

```js
module.exports = {
  // routes for your angular app. mirrors your routes file
  routes: [
    "/",
    "/about"
  ],
  
  // or getRoutes() for async routes you need to get first.
  getRoutes() {
    return someService.getMyRoutes()
  },
  // output folder for your site
  outputPath: "site", 
  // path to the compiled Server NgModule or NgModuleFactory with the #ExportName of the module
  serverModuleOrFactoryPath: "./dist/bundle#AppServerModule",
  // path to the root html page all pages will be injected into
  indexHTMLPath: "./src/index.html"
}
```

After that is all done, add a `script` in your `package.json` to build the static site using the cli:

```json
{
  "scripts": {
    "universal": "universal build"
  }
}
```

The `universal build` command will build your app as a static site, and output the html files to the outpath you specified in `universal.js`.


#### Programmatically (not complete)
Follow the common steps above first. You need to create an entry file for the site generator:

```typescript
// static.ts
import { generateSite } from '@angularclass/universal-sitegen'
import { AppServerModule } from './server.module'

generateSite(
  // NgModule or NgModuleFactory for the ServerModule you created
  AppServerModule,
  // index html file for all routes
  require('./index.html'),
  // options object
  {
    // routes for your app
    routes: [
      '/',
      '/about'
    ],
    // path to output the site
    outputPath: 'universal-site'
  }
)
.then(() => console.log('site built'))
```

Once that is done, build your app like you normally would. Because your static app will over ever be ran in Node, you can change your webpack config. Check out the [demo webpack config](https://github.com/angularclass/universal-sitegen/tree/master/demo/webpack.config.js)

After you build it, all you have to do is execute the bundled file.

```
node dist/bundle.js
```

This will generate your static site.

___

enjoy — **AngularClass**

<br><br>

[![AngularClass](https://cloud.githubusercontent.com/assets/1016365/9863770/cb0620fc-5af7-11e5-89df-d4b0b2cdfc43.png  "Angular Class")](https://angularclass.com)
## [AngularClass](https://angularclass.com)
> Learn AngularJS, Angular, and Modern Web Development from the best.
> Looking for corporate Angular training, want to host us, or Angular consulting? patrick@angularclass.com

___
