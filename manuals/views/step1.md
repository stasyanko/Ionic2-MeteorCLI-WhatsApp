[{]: <region> (header)
# Step 1: Bootstraping
[}]: #
[{]: <region> (body)
Both [Meteor](meteor.com) and [Ionic](ionicframework.com) took their platform to the next level in tooling.
Both provide CLI interface instead of bringing bunch of dependencies and configure build tools.
There are also differences between those tools. in this post we will focus on the `Meteor` CLI.

Start by installing `Meteor` if you haven't already (See [reference](https://www.meteor.com/install)).

Now let's create our app - write this in the command line:

    $ meteor create whatsapp

And let's see what we've got. Go into the new folder:

    $ cd whatsapp

A `Meteor` project will contain the following dirs by default:

  - client - A dir containing all client scripts.
  - server - A dir containing all server scripts.

These scripts should be loaded automatically by their alphabetic order on their belonging platform, e.g. a script defined under the client dir should be loaded by `Meteor` only on the client. A script defined in neither of these folders should be loaded on both.

`Meteor` apps use [Blaze](http://blazejs.org/) as its default template engine and [Babel](https://babeljs.io/) as its default script pre-processor. In this tutorial, we're interested in using [Angular 2](https://angular.io/)'s template engine and [Typescript](https://www.typescriptlang.org/) as our script pre-processor; Therefore will remove the `blaze-html-templates` package:

    $ meteor remove blaze-html-templates

And we will replace it with a package called `angular2-compilers`:

    $ meteor add angular2-compilers

The `angular2-compilers` package not only replace the `Blaze` template engine, but it also applies `Typescript` to our `Meteor` project, as it's the recommended scripting language recommended by the `Angular` team. In addition, all `CSS` files will be compiled with a pre-processor called [SASS](http://sass-lang.com/), something which will definitely ease the styling process.

The `Typescript` compiler operates based on a user defined config, and without it, it's not going to behave expectedly; Therefore, we will define the following `Typescript` config file:

[{]: <helper> (diff_step 1.3)
#### Step 1.3: Add Typescript config file

##### Added tsconfig.json
```diff
@@ -0,0 +1,38 @@
+┊  ┊ 1┊{
+┊  ┊ 2┊  "compilerOptions": {
+┊  ┊ 3┊    "allowSyntheticDefaultImports": true,
+┊  ┊ 4┊    "baseUrl": ".",
+┊  ┊ 5┊    "declaration": false,
+┊  ┊ 6┊    "emitDecoratorMetadata": true,
+┊  ┊ 7┊    "experimentalDecorators": true,
+┊  ┊ 8┊    "lib": [
+┊  ┊ 9┊      "dom",
+┊  ┊10┊      "es2015"
+┊  ┊11┊    ],
+┊  ┊12┊    "module": "commonjs",
+┊  ┊13┊    "moduleResolution": "node",
+┊  ┊14┊    "sourceMap": true,
+┊  ┊15┊    "target": "es5",
+┊  ┊16┊    "skipLibCheck": true,
+┊  ┊17┊    "stripInternal": true,
+┊  ┊18┊    "noImplicitAny": false,
+┊  ┊19┊    "types": [
+┊  ┊20┊      "meteor-typings",
+┊  ┊21┊      "@types/underscore",
+┊  ┊22┊      "@types/meteor-accounts-phone",
+┊  ┊23┊      "@types/meteor-publish-composite"
+┊  ┊24┊    ]
+┊  ┊25┊  },
+┊  ┊26┊  "include": [
+┊  ┊27┊    "client/**/*.ts",
+┊  ┊28┊    "server/**/*.ts",
+┊  ┊29┊    "imports/**/*.ts"
+┊  ┊30┊  ],
+┊  ┊31┊  "exclude": [
+┊  ┊32┊    "node_modules"
+┊  ┊33┊  ],
+┊  ┊34┊  "compileOnSave": false,
+┊  ┊35┊  "atom": {
+┊  ┊36┊    "rewriteTsconfig": false
+┊  ┊37┊  }
+┊  ┊38┊}🚫↵
```
[}]: #

> More information regards `Typescript` configuration file can be found [here](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html).

Not all third-party libraries have any support for `Typescript` whatsoever, something that we should be careful with when building a `Typescript` app. To allow non-supported third-party libraries, we will add the following declaration file:

[{]: <helper> (diff_step 1.4)
#### Step 1.4: Add declarations file

##### Added declarations.d.ts
```diff
@@ -0,0 +1 @@
+┊ ┊1┊declare module '*';🚫↵
```
[}]: #

Like most libraries, `Angular 2` is relied on peer dependencies, which we will have to make sure exist in our app by installing the following packages:

    $ meteor npm install --save @angular/common
    $ meteor npm install --save @angular/compiler
    $ meteor npm install --save @angular/compiler-cli
    $ meteor npm install --save @angular/core
    $ meteor npm install --save @angular/forms
    $ meteor npm install --save @angular/http
    $ meteor npm install --save @angular/platform-browser
    $ meteor npm install --save @angular/platform-browser-dynamic
    $ meteor npm install --save @angular/platform-server
    $ meteor npm install --save meteor-rxjs
    $ meteor npm install --save reflect-metadata
    $ meteor npm install --save rxjs
    $ meteor npm install --save zone.js
    $ meteor npm install --save-dev @types/meteor
    $ meteor npm install --save-dev @types/meteor-accounts-phone
    $ meteor npm install --save-dev @types/underscore
    $ meteor npm install --save-dev meteor-typings

Now that we have all the compilers and their dependencies ready, we will have to transform our project files into their supported extension. `.js` files should be renamed to `.ts` files, and `.css` files should be renamed to `.scss` files.

    $ mv server/main.js server/main.ts
    $ mv client/main.js client/main.ts
    $ mv client/main.css client/main.scss

Everything is set, and we can start using the `Angular 2` framework. We will start by setting the basis for our app:

[{]: <helper> (diff_step 1.7)
#### Step 1.7: Prepare application basis

##### Added client/imports/app/app.component.ts
```diff
@@ -0,0 +1,8 @@
+┊ ┊1┊import { Component } from '@angular/core';
+┊ ┊2┊import template from "./app.html";
+┊ ┊3┊
+┊ ┊4┊@Component({
+┊ ┊5┊  selector: 'my-app',
+┊ ┊6┊  template
+┊ ┊7┊})
+┊ ┊8┊export class MyApp {}
```

##### Added client/imports/app/app.html
```diff
@@ -0,0 +1 @@
+┊ ┊1┊My App🚫↵
```

##### Added client/imports/app/app.module.ts
```diff
@@ -0,0 +1,15 @@
+┊  ┊ 1┊import { NgModule, ErrorHandler } from '@angular/core';
+┊  ┊ 2┊import { MyApp } from './app.component';
+┊  ┊ 3┊
+┊  ┊ 4┊@NgModule({
+┊  ┊ 5┊  declarations: [
+┊  ┊ 6┊    MyApp
+┊  ┊ 7┊  ],
+┊  ┊ 8┊  entryComponents: [
+┊  ┊ 9┊    MyApp
+┊  ┊10┊  ],
+┊  ┊11┊  providers: [
+┊  ┊12┊    { provide: ErrorHandler }
+┊  ┊13┊  ]
+┊  ┊14┊})
+┊  ┊15┊export class AppModule {}🚫↵
```

##### Added client/imports/app/app.scss
```diff
@@ -0,0 +1,6 @@
+┊ ┊1┊// App Global Sass
+┊ ┊2┊// --------------------------------------------------
+┊ ┊3┊// Put style rules here that you want to apply globally. These
+┊ ┊4┊// styles are for the entire app and not just one component.
+┊ ┊5┊// Additionally, this file can be also used as an entry point
+┊ ┊6┊// to import other Sass files to be included in the output CSS.🚫↵
```

##### Changed client/main.html
```diff
@@ -3,23 +3,5 @@
 ┊ 3┊ 3┊</head>
 ┊ 4┊ 4┊
 ┊ 5┊ 5┊<body>
-┊ 6┊  ┊  <h1>Welcome to Meteor!</h1>
-┊ 7┊  ┊
-┊ 8┊  ┊  {{> hello}}
-┊ 9┊  ┊  {{> info}}
-┊10┊  ┊</body>
-┊11┊  ┊
-┊12┊  ┊<template name="hello">
-┊13┊  ┊  <button>Click Me</button>
-┊14┊  ┊  <p>You've pressed the button {{counter}} times.</p>
-┊15┊  ┊</template>
-┊16┊  ┊
-┊17┊  ┊<template name="info">
-┊18┊  ┊  <h2>Learn Meteor!</h2>
-┊19┊  ┊  <ul>
-┊20┊  ┊    <li><a href="https://www.meteor.com/try" target="_blank">Do the Tutorial</a></li>
-┊21┊  ┊    <li><a href="http://guide.meteor.com" target="_blank">Follow the Guide</a></li>
-┊22┊  ┊    <li><a href="https://docs.meteor.com" target="_blank">Read the Docs</a></li>
-┊23┊  ┊    <li><a href="https://forums.meteor.com" target="_blank">Discussions</a></li>
-┊24┊  ┊  </ul>
-┊25┊  ┊</template>
+┊  ┊ 6┊  <my-app></my-app>
+┊  ┊ 7┊</body>🚫↵
```

##### Changed client/main.scss
```diff
@@ -1 +1,2 @@
-┊1┊ ┊/* CSS declarations go here */
+┊ ┊1┊// App
+┊ ┊2┊@import "imports/app/app";🚫↵
```

##### Changed client/main.ts
```diff
@@ -1,22 +1,10 @@
-┊ 1┊  ┊import { Template } from 'meteor/templating';
-┊ 2┊  ┊import { ReactiveVar } from 'meteor/reactive-var';
+┊  ┊ 1┊import 'zone.js';
+┊  ┊ 2┊import 'reflect-metadata';
 ┊ 3┊ 3┊
-┊ 4┊  ┊import './main.html';
+┊  ┊ 4┊import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
+┊  ┊ 5┊import { Meteor } from 'meteor/meteor';
+┊  ┊ 6┊import { AppModule } from './imports/app/app.module';
 ┊ 5┊ 7┊
-┊ 6┊  ┊Template.hello.onCreated(function helloOnCreated() {
-┊ 7┊  ┊  // counter starts at 0
-┊ 8┊  ┊  this.counter = new ReactiveVar(0);
-┊ 9┊  ┊});
-┊10┊  ┊
-┊11┊  ┊Template.hello.helpers({
-┊12┊  ┊  counter() {
-┊13┊  ┊    return Template.instance().counter.get();
-┊14┊  ┊  },
-┊15┊  ┊});
-┊16┊  ┊
-┊17┊  ┊Template.hello.events({
-┊18┊  ┊  'click button'(event, instance) {
-┊19┊  ┊    // increment the counter when button is clicked
-┊20┊  ┊    instance.counter.set(instance.counter.get() + 1);
-┊21┊  ┊  },
+┊  ┊ 8┊Meteor.startup(() => {
+┊  ┊ 9┊  platformBrowserDynamic().bootstrapModule(AppModule);
 ┊22┊10┊});
```

##### Changed declarations.d.ts
```diff
@@ -1 +1,5 @@
+┊ ┊1┊/*
+┊ ┊2┊  A wildcard module is declared below to allow third party libraries to be used in an app even if they don't
+┊ ┊3┊  provide their own type declarations.
+┊ ┊4┊ */
 ┊1┊5┊declare module '*';🚫↵
```
[}]: #

In the step above, we simply created the entry module and component for our application, so as we go further in this tutorial and add more features, we can simply easily extend our module and app main component.

## Ionic 2

[Ionic](http://ionicframework.com/docs/) is a free and open source mobile SDK for developing native and progressive web apps with ease. When using `Ionic`CLI, it comes with a boilerplate, just like `Meteor` does, but since we're not using `Meteor` CLI and not `Ionic`'s, we will have to set it up manually.

The first thing we're going to do in order to integrate `Ionic 2` in our app would be installing its `NPM` dependencies:

    $ meteor npm install --save @ionic/storage
    $ meteor npm install --save ionic-angular
    $ meteor npm install --save ionic-native
    $ meteor npm install --save ionicons

`Ionic` build system comes with a built-in theming system which helps its users design their app. It's a powerful tool which we wanna take advantage of. In-order to do that, we will define the following `SCSS` file, and we will import it:

[{]: <helper> (diff_step 1.9)
#### Step 1.9: Add theme variables

##### Added client/imports/theme/variables.scss
```diff
@@ -0,0 +1,308 @@
+┊   ┊  1┊// Ionic Variables and Theming. For more info, please see:
+┊   ┊  2┊// http://ionicframework.com/docs/v2/theming/
+┊   ┊  3┊$font-path: "{}/node_modules/ionicons/dist/fonts";
+┊   ┊  4┊
+┊   ┊  5┊@import "{}/node_modules/ionic-angular/themes/ionic.globals";
+┊   ┊  6┊
+┊   ┊  7┊
+┊   ┊  8┊// Shared Variables
+┊   ┊  9┊// --------------------------------------------------
+┊   ┊ 10┊// To customize the look and feel of this app, you can override
+┊   ┊ 11┊// the Sass variables found in Ionic's source scss files.
+┊   ┊ 12┊// To view all the possible Ionic variables, see:
+┊   ┊ 13┊// http://ionicframework.com/docs/v2/theming/overriding-ionic-variables/
+┊   ┊ 14┊
+┊   ┊ 15┊
+┊   ┊ 16┊
+┊   ┊ 17┊
+┊   ┊ 18┊// Named Color Variables
+┊   ┊ 19┊// --------------------------------------------------
+┊   ┊ 20┊// Named colors makes it easy to reuse colors on various components.
+┊   ┊ 21┊// It's highly recommended to change the default colors
+┊   ┊ 22┊// to match your app's branding. Ionic uses a Sass map of
+┊   ┊ 23┊// colors so you can add, rename and remove colors as needed.
+┊   ┊ 24┊// The "primary" color is the only required color in the map.
+┊   ┊ 25┊
+┊   ┊ 26┊$colors: (
+┊   ┊ 27┊  primary:    #387ef5,
+┊   ┊ 28┊  secondary:  #32db64,
+┊   ┊ 29┊  danger:     #f53d3d,
+┊   ┊ 30┊  light:      #f4f4f4,
+┊   ┊ 31┊  dark:       #222
+┊   ┊ 32┊);
+┊   ┊ 33┊
+┊   ┊ 34┊// Components
+┊   ┊ 35┊// --------------------------------------------------
+┊   ┊ 36┊
+┊   ┊ 37┊@import
+┊   ┊ 38┊"{}/node_modules/ionic-angular/components/action-sheet/action-sheet",
+┊   ┊ 39┊"{}/node_modules/ionic-angular/components/action-sheet/action-sheet.ios",
+┊   ┊ 40┊"{}/node_modules/ionic-angular/components/action-sheet/action-sheet.md",
+┊   ┊ 41┊"{}/node_modules/ionic-angular/components/action-sheet/action-sheet.wp";
+┊   ┊ 42┊
+┊   ┊ 43┊@import
+┊   ┊ 44┊"{}/node_modules/ionic-angular/components/alert/alert",
+┊   ┊ 45┊"{}/node_modules/ionic-angular/components/alert/alert.ios",
+┊   ┊ 46┊"{}/node_modules/ionic-angular/components/alert/alert.md",
+┊   ┊ 47┊"{}/node_modules/ionic-angular/components/alert/alert.wp";
+┊   ┊ 48┊
+┊   ┊ 49┊@import
+┊   ┊ 50┊"{}/node_modules/ionic-angular/components/app/app",
+┊   ┊ 51┊"{}/node_modules/ionic-angular/components/app/app.ios",
+┊   ┊ 52┊"{}/node_modules/ionic-angular/components/app/app.md",
+┊   ┊ 53┊"{}/node_modules/ionic-angular/components/app/app.wp";
+┊   ┊ 54┊
+┊   ┊ 55┊@import
+┊   ┊ 56┊"{}/node_modules/ionic-angular/components/backdrop/backdrop";
+┊   ┊ 57┊
+┊   ┊ 58┊@import
+┊   ┊ 59┊"{}/node_modules/ionic-angular/components/badge/badge",
+┊   ┊ 60┊"{}/node_modules/ionic-angular/components/badge/badge.ios",
+┊   ┊ 61┊"{}/node_modules/ionic-angular/components/badge/badge.md",
+┊   ┊ 62┊"{}/node_modules/ionic-angular/components/badge/badge.wp";
+┊   ┊ 63┊
+┊   ┊ 64┊@import
+┊   ┊ 65┊"{}/node_modules/ionic-angular/components/button/button",
+┊   ┊ 66┊"{}/node_modules/ionic-angular/components/button/button.ios",
+┊   ┊ 67┊"{}/node_modules/ionic-angular/components/button/button.md",
+┊   ┊ 68┊"{}/node_modules/ionic-angular/components/button/button.wp";
+┊   ┊ 69┊
+┊   ┊ 70┊@import
+┊   ┊ 71┊"{}/node_modules/ionic-angular/components/card/card",
+┊   ┊ 72┊"{}/node_modules/ionic-angular/components/card/card.ios",
+┊   ┊ 73┊"{}/node_modules/ionic-angular/components/card/card.md",
+┊   ┊ 74┊"{}/node_modules/ionic-angular/components/card/card.wp";
+┊   ┊ 75┊
+┊   ┊ 76┊@import
+┊   ┊ 77┊"{}/node_modules/ionic-angular/components/checkbox/checkbox.ios",
+┊   ┊ 78┊"{}/node_modules/ionic-angular/components/checkbox/checkbox.md",
+┊   ┊ 79┊"{}/node_modules/ionic-angular/components/checkbox/checkbox.wp";
+┊   ┊ 80┊
+┊   ┊ 81┊@import
+┊   ┊ 82┊"{}/node_modules/ionic-angular/components/chip/chip",
+┊   ┊ 83┊"{}/node_modules/ionic-angular/components/chip/chip.ios",
+┊   ┊ 84┊"{}/node_modules/ionic-angular/components/chip/chip.md",
+┊   ┊ 85┊"{}/node_modules/ionic-angular/components/chip/chip.wp";
+┊   ┊ 86┊
+┊   ┊ 87┊@import
+┊   ┊ 88┊"{}/node_modules/ionic-angular/components/content/content",
+┊   ┊ 89┊"{}/node_modules/ionic-angular/components/content/content.ios",
+┊   ┊ 90┊"{}/node_modules/ionic-angular/components/content/content.md",
+┊   ┊ 91┊"{}/node_modules/ionic-angular/components/content/content.wp";
+┊   ┊ 92┊
+┊   ┊ 93┊@import
+┊   ┊ 94┊"{}/node_modules/ionic-angular/components/datetime/datetime",
+┊   ┊ 95┊"{}/node_modules/ionic-angular/components/datetime/datetime.ios",
+┊   ┊ 96┊"{}/node_modules/ionic-angular/components/datetime/datetime.md",
+┊   ┊ 97┊"{}/node_modules/ionic-angular/components/datetime/datetime.wp";
+┊   ┊ 98┊
+┊   ┊ 99┊@import
+┊   ┊100┊"{}/node_modules/ionic-angular/components/fab/fab",
+┊   ┊101┊"{}/node_modules/ionic-angular/components/fab/fab.ios",
+┊   ┊102┊"{}/node_modules/ionic-angular/components/fab/fab.md",
+┊   ┊103┊"{}/node_modules/ionic-angular/components/fab/fab.wp";
+┊   ┊104┊
+┊   ┊105┊@import
+┊   ┊106┊"{}/node_modules/ionic-angular/components/grid/grid";
+┊   ┊107┊
+┊   ┊108┊@import
+┊   ┊109┊"{}/node_modules/ionic-angular/components/icon/icon",
+┊   ┊110┊"{}/node_modules/ionic-angular/components/icon/icon.ios",
+┊   ┊111┊"{}/node_modules/ionic-angular/components/icon/icon.md",
+┊   ┊112┊"{}/node_modules/ionic-angular/components/icon/icon.wp";
+┊   ┊113┊
+┊   ┊114┊@import
+┊   ┊115┊"{}/node_modules/ionic-angular/components/img/img";
+┊   ┊116┊
+┊   ┊117┊@import
+┊   ┊118┊"{}/node_modules/ionic-angular/components/infinite-scroll/infinite-scroll";
+┊   ┊119┊
+┊   ┊120┊@import
+┊   ┊121┊"{}/node_modules/ionic-angular/components/input/input",
+┊   ┊122┊"{}/node_modules/ionic-angular/components/input/input.ios",
+┊   ┊123┊"{}/node_modules/ionic-angular/components/input/input.md",
+┊   ┊124┊"{}/node_modules/ionic-angular/components/input/input.wp";
+┊   ┊125┊
+┊   ┊126┊@import
+┊   ┊127┊"{}/node_modules/ionic-angular/components/item/item",
+┊   ┊128┊"{}/node_modules/ionic-angular/components/item/item.ios",
+┊   ┊129┊"{}/node_modules/ionic-angular/components/item/item.md",
+┊   ┊130┊"{}/node_modules/ionic-angular/components/item/item.wp";
+┊   ┊131┊
+┊   ┊132┊@import
+┊   ┊133┊"{}/node_modules/ionic-angular/components/label/label",
+┊   ┊134┊"{}/node_modules/ionic-angular/components/label/label.ios",
+┊   ┊135┊"{}/node_modules/ionic-angular/components/label/label.md",
+┊   ┊136┊"{}/node_modules/ionic-angular/components/label/label.wp";
+┊   ┊137┊
+┊   ┊138┊@import
+┊   ┊139┊"{}/node_modules/ionic-angular/components/list/list",
+┊   ┊140┊"{}/node_modules/ionic-angular/components/list/list.ios",
+┊   ┊141┊"{}/node_modules/ionic-angular/components/list/list.md",
+┊   ┊142┊"{}/node_modules/ionic-angular/components/list/list.wp";
+┊   ┊143┊
+┊   ┊144┊@import
+┊   ┊145┊"{}/node_modules/ionic-angular/components/loading/loading",
+┊   ┊146┊"{}/node_modules/ionic-angular/components/loading/loading.ios",
+┊   ┊147┊"{}/node_modules/ionic-angular/components/loading/loading.md",
+┊   ┊148┊"{}/node_modules/ionic-angular/components/loading/loading.wp";
+┊   ┊149┊
+┊   ┊150┊@import
+┊   ┊151┊"{}/node_modules/ionic-angular/components/menu/menu",
+┊   ┊152┊"{}/node_modules/ionic-angular/components/menu/menu.ios",
+┊   ┊153┊"{}/node_modules/ionic-angular/components/menu/menu.md",
+┊   ┊154┊"{}/node_modules/ionic-angular/components/menu/menu.wp";
+┊   ┊155┊
+┊   ┊156┊@import
+┊   ┊157┊"{}/node_modules/ionic-angular/components/modal/modal",
+┊   ┊158┊"{}/node_modules/ionic-angular/components/modal/modal.ios",
+┊   ┊159┊"{}/node_modules/ionic-angular/components/modal/modal.md",
+┊   ┊160┊"{}/node_modules/ionic-angular/components/modal/modal.wp";
+┊   ┊161┊
+┊   ┊162┊@import
+┊   ┊163┊"{}/node_modules/ionic-angular/components/note/note.ios",
+┊   ┊164┊"{}/node_modules/ionic-angular/components/note/note.md",
+┊   ┊165┊"{}/node_modules/ionic-angular/components/note/note.wp";
+┊   ┊166┊
+┊   ┊167┊@import
+┊   ┊168┊"{}/node_modules/ionic-angular/components/picker/picker",
+┊   ┊169┊"{}/node_modules/ionic-angular/components/picker/picker.ios",
+┊   ┊170┊"{}/node_modules/ionic-angular/components/picker/picker.md",
+┊   ┊171┊"{}/node_modules/ionic-angular/components/picker/picker.wp";
+┊   ┊172┊
+┊   ┊173┊@import
+┊   ┊174┊"{}/node_modules/ionic-angular/components/popover/popover",
+┊   ┊175┊"{}/node_modules/ionic-angular/components/popover/popover.ios",
+┊   ┊176┊"{}/node_modules/ionic-angular/components/popover/popover.md",
+┊   ┊177┊"{}/node_modules/ionic-angular/components/popover/popover.wp";
+┊   ┊178┊
+┊   ┊179┊@import
+┊   ┊180┊"{}/node_modules/ionic-angular/components/radio/radio.md",
+┊   ┊181┊"{}/node_modules/ionic-angular/components/radio/radio.wp";
+┊   ┊182┊
+┊   ┊183┊@import
+┊   ┊184┊"{}/node_modules/ionic-angular/components/range/range",
+┊   ┊185┊"{}/node_modules/ionic-angular/components/range/range.ios",
+┊   ┊186┊"{}/node_modules/ionic-angular/components/range/range.md",
+┊   ┊187┊"{}/node_modules/ionic-angular/components/range/range.wp";
+┊   ┊188┊
+┊   ┊189┊@import
+┊   ┊190┊"{}/node_modules/ionic-angular/components/refresher/refresher";
+┊   ┊191┊
+┊   ┊192┊@import
+┊   ┊193┊"{}/node_modules/ionic-angular/components/scroll/scroll";
+┊   ┊194┊
+┊   ┊195┊@import
+┊   ┊196┊"{}/node_modules/ionic-angular/components/searchbar/searchbar",
+┊   ┊197┊"{}/node_modules/ionic-angular/components/searchbar/searchbar.ios",
+┊   ┊198┊"{}/node_modules/ionic-angular/components/searchbar/searchbar.md",
+┊   ┊199┊"{}/node_modules/ionic-angular/components/searchbar/searchbar.wp";
+┊   ┊200┊
+┊   ┊201┊@import
+┊   ┊202┊"{}/node_modules/ionic-angular/components/segment/segment",
+┊   ┊203┊"{}/node_modules/ionic-angular/components/segment/segment.ios",
+┊   ┊204┊"{}/node_modules/ionic-angular/components/segment/segment.md",
+┊   ┊205┊"{}/node_modules/ionic-angular/components/segment/segment.wp";
+┊   ┊206┊
+┊   ┊207┊@import
+┊   ┊208┊"{}/node_modules/ionic-angular/components/select/select",
+┊   ┊209┊"{}/node_modules/ionic-angular/components/select/select.ios",
+┊   ┊210┊"{}/node_modules/ionic-angular/components/select/select.md",
+┊   ┊211┊"{}/node_modules/ionic-angular/components/select/select.wp";
+┊   ┊212┊
+┊   ┊213┊@import
+┊   ┊214┊"{}/node_modules/ionic-angular/components/show-hide-when/show-hide-when";
+┊   ┊215┊
+┊   ┊216┊@import
+┊   ┊217┊"{}/node_modules/ionic-angular/components/slides/slides";
+┊   ┊218┊
+┊   ┊219┊@import
+┊   ┊220┊"{}/node_modules/ionic-angular/components/spinner/spinner",
+┊   ┊221┊"{}/node_modules/ionic-angular/components/spinner/spinner.ios",
+┊   ┊222┊"{}/node_modules/ionic-angular/components/spinner/spinner.md",
+┊   ┊223┊"{}/node_modules/ionic-angular/components/spinner/spinner.wp";
+┊   ┊224┊
+┊   ┊225┊@import
+┊   ┊226┊"{}/node_modules/ionic-angular/components/tabs/tabs",
+┊   ┊227┊"{}/node_modules/ionic-angular/components/tabs/tabs.ios",
+┊   ┊228┊"{}/node_modules/ionic-angular/components/tabs/tabs.md",
+┊   ┊229┊"{}/node_modules/ionic-angular/components/tabs/tabs.wp";
+┊   ┊230┊
+┊   ┊231┊@import
+┊   ┊232┊"{}/node_modules/ionic-angular/components/toast/toast",
+┊   ┊233┊"{}/node_modules/ionic-angular/components/toast/toast.ios",
+┊   ┊234┊"{}/node_modules/ionic-angular/components/toast/toast.md",
+┊   ┊235┊"{}/node_modules/ionic-angular/components/toast/toast.wp";
+┊   ┊236┊
+┊   ┊237┊@import
+┊   ┊238┊"{}/node_modules/ionic-angular/components/toggle/toggle.ios",
+┊   ┊239┊"{}/node_modules/ionic-angular/components/toggle/toggle.md",
+┊   ┊240┊"{}/node_modules/ionic-angular/components/toggle/toggle.wp";
+┊   ┊241┊
+┊   ┊242┊@import
+┊   ┊243┊"{}/node_modules/ionic-angular/components/toolbar/toolbar",
+┊   ┊244┊"{}/node_modules/ionic-angular/components/toolbar/toolbar.ios",
+┊   ┊245┊"{}/node_modules/ionic-angular/components/toolbar/toolbar.md",
+┊   ┊246┊"{}/node_modules/ionic-angular/components/toolbar/toolbar.wp";
+┊   ┊247┊
+┊   ┊248┊@import
+┊   ┊249┊"{}/node_modules/ionic-angular/components/typography/typography",
+┊   ┊250┊"{}/node_modules/ionic-angular/components/typography/typography.ios",
+┊   ┊251┊"{}/node_modules/ionic-angular/components/typography/typography.md",
+┊   ┊252┊"{}/node_modules/ionic-angular/components/typography/typography.wp";
+┊   ┊253┊
+┊   ┊254┊
+┊   ┊255┊// Platforms
+┊   ┊256┊// --------------------------------------------------
+┊   ┊257┊@import
+┊   ┊258┊"{}/node_modules/ionic-angular/platform/cordova",
+┊   ┊259┊"{}/node_modules/ionic-angular/platform/cordova.ios",
+┊   ┊260┊"{}/node_modules/ionic-angular/platform/cordova.md",
+┊   ┊261┊"{}/node_modules/ionic-angular/platform/cordova.wp";
+┊   ┊262┊
+┊   ┊263┊
+┊   ┊264┊
+┊   ┊265┊// App iOS Variables
+┊   ┊266┊// --------------------------------------------------
+┊   ┊267┊// iOS only Sass variables can go here
+┊   ┊268┊
+┊   ┊269┊
+┊   ┊270┊
+┊   ┊271┊
+┊   ┊272┊// App Material Design Variables
+┊   ┊273┊// --------------------------------------------------
+┊   ┊274┊// Material Design only Sass variables can go here
+┊   ┊275┊
+┊   ┊276┊
+┊   ┊277┊
+┊   ┊278┊
+┊   ┊279┊// App Windows Variables
+┊   ┊280┊// --------------------------------------------------
+┊   ┊281┊// Windows only Sass variables can go here
+┊   ┊282┊
+┊   ┊283┊
+┊   ┊284┊
+┊   ┊285┊
+┊   ┊286┊// App Theme
+┊   ┊287┊// --------------------------------------------------
+┊   ┊288┊// Ionic apps can have different themes applied, which can
+┊   ┊289┊// then be future customized. This import comes last
+┊   ┊290┊// so that the above variables are used and Ionic's
+┊   ┊291┊// default are overridden.
+┊   ┊292┊
+┊   ┊293┊@import "{}/node_modules/ionic-angular/themes/ionic.theme.default";
+┊   ┊294┊
+┊   ┊295┊
+┊   ┊296┊// Ionicons
+┊   ┊297┊// --------------------------------------------------
+┊   ┊298┊// The premium icon font for Ionic. For more info, please see:
+┊   ┊299┊// http://ionicframework.com/docs/v2/ionicons/
+┊   ┊300┊
+┊   ┊301┊@import "{}/node_modules/ionicons/dist/scss/ionicons-icons";
+┊   ┊302┊
+┊   ┊303┊
+┊   ┊304┊// Fonts
+┊   ┊305┊// --------------------------------------------------
+┊   ┊306┊
+┊   ┊307┊@import "{}/node_modules/ionic-angular/fonts/roboto";
+┊   ┊308┊@import "{}/node_modules/ionic-angular/fonts/noto-sans";🚫↵
```

##### Changed client/main.scss
```diff
@@ -1,2 +1,5 @@
+┊ ┊1┊// Theme
+┊ ┊2┊@import "imports/theme/variables";
+┊ ┊3┊
 ┊1┊4┊// App
 ┊2┊5┊@import "imports/app/app";🚫↵
```
[}]: #

> The `variables.scss` file is hooked to different `Ionic` packages located in the `node_modules` dir. It means that the logic already existed, we only bound all the necessary modules together in order to emulate `Ionic`'s theming system.

`Ionic` looks for fonts in directory we can't access. To fix it, we will use the `mys:font` package to teach `Meteor` how to put them there.

    $ meteor add mys:fonts

That plugin needs to know which font we want to use and where it should be available.

Configuration is pretty easy, you will catch it by just looking on an example:

[{]: <helper> (diff_step 1.11)
#### Step 1.11: Add fonts mapping file

##### Added fonts.json
```diff
@@ -0,0 +1,17 @@
+┊  ┊ 1┊{
+┊  ┊ 2┊  "map": {
+┊  ┊ 3┊    "node_modules/ionicons/dist/fonts/ionicons.ttf": "fonts/ionicons.ttf",
+┊  ┊ 4┊    "node_modules/ionicons/dist/fonts/ionicons.woff": "fonts/ionicons.woff",
+┊  ┊ 5┊    "node_modules/ionicons/dist/fonts/ionicons.woff2": "fonts/ionicons.woff2",
+┊  ┊ 6┊    "node_modules/ionicons/dist/fonts/noto-sans-bold.ttf": "fonts/noto-sans-bold.ttf",
+┊  ┊ 7┊    "node_modules/ionicons/dist/fonts/noto-sans-regular.ttf": "fonts/noto-sans-regular.ttf",
+┊  ┊ 8┊    "node_modules/ionicons/dist/fonts/roboto-bold.ttf": "fonts/roboto-bold.ttf",
+┊  ┊ 9┊    "node_modules/ionicons/dist/fonts/roboto-bold.woff": "fonts/roboto-bold.woff",
+┊  ┊10┊    "node_modules/ionicons/dist/fonts/roboto-light.ttf": "fonts/roboto-light.ttf",
+┊  ┊11┊    "node_modules/ionicons/dist/fonts/roboto-light.woff": "fonts/roboto-light.woff",
+┊  ┊12┊    "node_modules/ionicons/dist/fonts/roboto-medium.ttf": "fonts/roboto-medium.ttf",
+┊  ┊13┊    "node_modules/ionicons/dist/fonts/roboto-medium.woff": "fonts/roboto-medium.woff",
+┊  ┊14┊    "node_modules/ionicons/dist/fonts/roboto-regular.ttf": "fonts/roboto-regular.ttf",
+┊  ┊15┊    "node_modules/ionicons/dist/fonts/roboto-regular.woff": "fonts/roboto-regular.woff"
+┊  ┊16┊  }
+┊  ┊17┊}🚫↵
```
[}]: #

`Ionic` is set. We will have to make few adjustments in our app in order to use `Ionic`, mostly importing its modules and using its components:

[{]: <helper> (diff_step 1.12)
#### Step 1.12: Add Ionic to application basis

##### Changed client/imports/app/app.component.ts
```diff
@@ -1,8 +1,20 @@
 ┊ 1┊ 1┊import { Component } from '@angular/core';
+┊  ┊ 2┊import { Platform } from 'ionic-angular';
+┊  ┊ 3┊import { StatusBar, Splashscreen } from 'ionic-native';
 ┊ 2┊ 4┊import template from "./app.html";
 ┊ 3┊ 5┊
 ┊ 4┊ 6┊@Component({
-┊ 5┊  ┊  selector: 'my-app',
 ┊ 6┊ 7┊  template
 ┊ 7┊ 8┊})
-┊ 8┊  ┊export class MyApp {}
+┊  ┊ 9┊export class MyApp {
+┊  ┊10┊  constructor(platform: Platform) {
+┊  ┊11┊    platform.ready().then(() => {
+┊  ┊12┊      // Okay, so the platform is ready and our plugins are available.
+┊  ┊13┊      // Here you can do any higher level native things you might need.
+┊  ┊14┊      if (platform.is('cordova')) {
+┊  ┊15┊        StatusBar.styleDefault();
+┊  ┊16┊        Splashscreen.hide();
+┊  ┊17┊      }
+┊  ┊18┊    });
+┊  ┊19┊  }
+┊  ┊20┊}🚫↵
```

##### Changed client/imports/app/app.module.ts
```diff
@@ -1,15 +1,20 @@
 ┊ 1┊ 1┊import { NgModule, ErrorHandler } from '@angular/core';
+┊  ┊ 2┊import { IonicApp, IonicModule, IonicErrorHandler } from 'ionic-angular';
 ┊ 2┊ 3┊import { MyApp } from './app.component';
 ┊ 3┊ 4┊
 ┊ 4┊ 5┊@NgModule({
 ┊ 5┊ 6┊  declarations: [
 ┊ 6┊ 7┊    MyApp
 ┊ 7┊ 8┊  ],
+┊  ┊ 9┊  imports: [
+┊  ┊10┊    IonicModule.forRoot(MyApp),
+┊  ┊11┊  ],
+┊  ┊12┊  bootstrap: [IonicApp],
 ┊ 8┊13┊  entryComponents: [
 ┊ 9┊14┊    MyApp
 ┊10┊15┊  ],
 ┊11┊16┊  providers: [
-┊12┊  ┊    { provide: ErrorHandler }
+┊  ┊17┊    { provide: ErrorHandler, useClass: IonicErrorHandler }
 ┊13┊18┊  ]
 ┊14┊19┊})
 ┊15┊20┊export class AppModule {}🚫↵
```

##### Changed client/main.html
```diff
@@ -1,7 +1,11 @@
 ┊ 1┊ 1┊<head>
 ┊ 2┊ 2┊  <title>Ionic2-MeteorCLI-WhatsApp</title>
+┊  ┊ 3┊  <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
+┊  ┊ 4┊  <meta name="format-detection" content="telephone=no">
+┊  ┊ 5┊  <meta name="msapplication-tap-highlight" content="no">
+┊  ┊ 6┊  <meta name="theme-color" content="#4e8ef7">
 ┊ 3┊ 7┊</head>
 ┊ 4┊ 8┊
 ┊ 5┊ 9┊<body>
-┊ 6┊  ┊  <my-app></my-app>
+┊  ┊10┊  <ion-app></ion-app>
 ┊ 7┊11┊</body>🚫↵
```
[}]: #

## Running on Mobile

To add mobile support, select the platform(s) you want and run the following command:

    $ meteor add-platform ios
    # OR / AND
    $ meteor add-platform android

To run an app in the emulator use:

    $ meteor run ios
    # OR
    $ meteor run android

To learn more about **Mobile** in `Meteor` read the [*"Mobile"* chapter](https://guide.meteor.com/mobile.html) of the Meteor Guide.

`Meteor` projects come with a package called `mobile-experience` by default, which is a bundle of `fastclick`, `mobile-status-bar` and `launch-screen`. The `fastclick` package might cause some conflicts between `Meteor` and `Ionic`'s functionality, something which will probably cause some unexpected behavior. To fix it, we're going to remove `mobile-experience` and install the rest of its packages explicitly:

    $ meteor remove mobile-experience
    $ meteor add mobile-status-bar
    $ meteor add launch-screen

### Web

`Ionic` apps are still usable in the browser. You can run them using the command:

    $ meteor start
    # OR
    $ npm start

The app should be running on port `3000`, and can be changed by specifying a `--port` option.
[}]: #
[{]: <region> (footer)
[{]: <helper> (nav_step)
| [< Intro](../../README.md) | [Next Step >](step2.md) |
|:--------------------------------|--------------------------------:|
[}]: #
[}]: #