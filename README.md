# AngularFire2 Offline [![npm version](https://badge.fury.io/js/angularfire2-offline.svg)](https://badge.fury.io/js/angularfire2-offline)

🔌 A simple wrapper for [AngularFire2](https://github.com/angular/angularfire2) to read and write to Firebase while offline, even after a complete refresh.

[![Build Status](http://img.shields.io/travis/adriancarriger/angularfire2-offline/master.svg?maxAge=60)](https://travis-ci.org/adriancarriger/angularfire2-offline)
[![Codecov](https://img.shields.io/codecov/c/github/adriancarriger/angularfire2-offline/master.svg?maxAge=60)](https://codecov.io/gh/adriancarriger/angularfire2-offline)
[![Dependency Status](https://img.shields.io/david/adriancarriger/angularfire2-offline/master.svg?maxAge=60)](https://david-dm.org/adriancarriger/angularfire2-offline)
[![devDependency Status](https://img.shields.io/david/dev/adriancarriger/angularfire2-offline/master.svg?maxAge=60)](https://david-dm.org/adriancarriger/angularfire2-offline?type=dev)
[![peerDependency Status](https://img.shields.io/david/peer/adriancarriger/angularfire2-offline/master.svg?maxAge=60)](https://david-dm.org/adriancarriger/angularfire2-offline?type=peer)

- [Complete changelog](https://github.com/adriancarriger/angularfire2-offline/releases)

## Demos

- [`Angular 2+ Demos:`](https://angularfire2-offline.firebaseapp.com/) [Read object](https://angularfire2-offline.firebaseapp.com/read-object), [Read list](https://angularfire2-offline.firebaseapp.com/read-list), [Write object](https://angularfire2-offline.firebaseapp.com/write-object), [Write list](https://angularfire2-offline.firebaseapp.com/write-list) -- [tutorial 📗](https://github.com/adriancarriger/angularfire2-offline/tree/master/examples/angular-cli#angular-cli-offline-tutorial-)
- [`Ionic 2+ Demo`](https://ionic-pwa-ad85b.firebaseapp.com) -- [tutorial 📘](https://github.com/adriancarriger/angularfire2-offline/tree/master/examples/ionic#ionic-offline-tutorial-)

[![Example Gif](https://raw.githubusercontent.com/adriancarriger/angularfire2-offline/master/images/example.gif)](https://angularfire2-offline.firebaseapp.com/write-list)

## Install

```bash
npm install angularfire2-offline angularfire2 firebase --save
```

## Setup @NgModule

```ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AngularFireModule } from 'angularfire2';
import { AngularFireOfflineModule } from 'angularfire2-offline';

import { AppComponent } from './app.component';

// Must export the config
export const firebaseConfig = {
  apiKey: '<your-key>',
  authDomain: '<your-project-authdomain>',
  databaseURL: '<your-database-URL>',
  storageBucket: '<your-storage-bucket>'
};

@NgModule({
  declarations: [AppComponent],
  imports: [
    AngularFireModule.initializeApp(firebaseConfig),
    AngularFireOfflineModule,
    BrowserModule
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## Usage

- Methods mirror AngularFire2 database methods for [`object`](https://github.com/angular/angularfire2/blob/master/docs/2-retrieving-data-as-objects.md#retrieve-data) and [`list`](https://github.com/angular/angularfire2/blob/master/docs/3-retrieving-data-as-lists.md#retrieve-data).

### Read Data Offline

- [Querying lists](https://github.com/angular/angularfire2/blob/master/docs/4-querying-lists.md) is supported

```ts
import { Component } from '@angular/core';
import {
  AfoListObservable,
  AfoObjectObservable,
  AngularFireOffline } from 'angularfire2-offline';

@Component({
  selector: 'project-name-app',
  template: `
  <h1>{{ (info | async)?.name }}</h1>
  <ul>
    <li *ngFor="let item of items | async">
      {{ item?.name }}
    </li>
  </ul>
  `
})
export class MyApp {
  info: AfoObjectObservable<any>;
  items: AfoListObservable<any[]>;
  constructor(afo: AngularFireOffline) {
    this.info = afo.database.object('/info');
    this.items = afo.database.list('/items');
  }
}
```

### Write data offline

If writes are made offline followed by a page refresh, the writes will be sent when a connection becomes available.

- [Write object](https://angularfire2-offline.firebaseapp.com/write-object)
- [Write list](https://angularfire2-offline.firebaseapp.com/write-list)
- [Resolve write conflicts](https://angularfire2-offline.firebaseapp.com/write-conflicts/messages)

## How it works

 - While online, Firebase data is stored locally (as data changes the local store is updated)
 - While offline, local data is served if available, and writes are stored locally
 - On reconnect, app updates with new Firebase data, and writes are sent to Firebase
 - Even while online, local data is used first when available which results in a faster load

## License

angularfire2-offline is licensed under the MIT Open Source license. For more information, see the [LICENSE](LICENSE) file in this repository.
