---
title: "How to upgrade Angular and Ionic in your Ionic v4 apps"
date: 2018-08-06T19:55:03-06:00
draft: false
---

**Note**: This blog is based off an early beta release of Ionic v4. Some code samples might change in subsequent releases.

**Update 8/8/2018**: As typical in this crazy JS world, a new drop of Ionic v4 beta 2 came out while working on this post. The new version includes Angular 6.1, but hopefully this will help you update future versions.

A couple weeks ago, the Ionic team released the beta of Ionic v4, which represents a major shift in the way that Ionic works. Part of the release was Ionic aligning itself better with the Angular ecosystem. Instead of providing a bunch of auxillary tooling, Ionic now relies on the underlying framework to provide a lot of these needed features. For instance, in Ionic v3, Ionic had its own build tools in the form of `ionic-app-scripts`, which were fired off from the Ionic CLI when doing Angular builds. Now, when running a `ionic build` or `ionic serve` command, the Ionic CLI is simply calling into the great Angular CLI to perform these commands.

Because Ionic is no longer as tightly coupled to Angular as it once was, it is now easier to upgrade Angular in an Ionic application. Just a couple of days after Ionic's first beta release, the Angular team dropped version 6.1 of Angular. While the Ionic templates will probably be updated soon to ng6.1, if you want to take advantage of some of the pretty cool features that come in 6.1, you can simply do so by running

```bash
ng update @angular/core
```
command in the root of your Ionic v4 app. This will automatically update all of the angular libs to the latest version (6.1 in our case) if its able to. It will also update to the latest supported TypeScript as well (2.9 in our case as well). You might get some warnings about peer dependecnies, but things should still work in your app.

Also, when new versions of Ionic drop, you can upgrade Ionic in your app in a similar fashion:

```bash
ng update @ionic/angular
```

There are few new features in Angular v6.1 to benefit Ionic developers. Unfortunately, the one I was looking the most forward to, the scroll position restoration, does not work with Ionic because Ionic has its own method of scrolling, and Angular relies on the windows scroll position. However, some other features include a keyValue pipe that will let you iterate over plain javascript objects and maps in your templates, as well as a TypeScript upgrade to 2.9.

