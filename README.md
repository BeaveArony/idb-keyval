# localstorage-IDB-Keyval

This is Jake Archibald's implementation slightly altered to adhere to the localStorage API. This way it can easily be used with projects like redux-persist.

[![npm](https://img.shields.io/npm/v/localstorage-idb-keyval.svg)](https://www.npmjs.com/package/localstorage-idb-keyval)
[![size](http://img.badgesize.io/https://cdn.jsdelivr.net/npm/localstorage-idb-keyval/dist/localstorage-idb-keyval-iife.min.js?compression=gzip)](http://img.badgesize.io/https://cdn.jsdelivr.net/npm/localstorage-idb-keyval/dist/localstorage-idb-keyval-iife.min.js)

This is a super-simple-small promise-based keyval store implemented with IndexedDB, largely based on [async-storage by Mozilla](https://github.com/mozilla-b2g/gaia/blob/master/shared/js/async_storage.js).

[localForage](https://github.com/localForage/localForage) offers similar functionality, but supports older browsers with broken/absent IDB implementations. Because of that, it's 7.4k, whereas localstorage-idb-keyval is ~550 bytes. Also, it's tree-shaking friendly, so you'll probably end up using fewer than 450 bytes. Pick whichever works best for you!

This is only a keyval store. If you need to do more complex things like iteration & indexing, check out [IDB on NPM](https://www.npmjs.com/package/idb) (a little heavier at 1.7k). The first example in its README is how to recreate this library.

## Usage

### setItem:

```js
import { setItem } from 'localstorage-idb-keyval';

setItem('hello', 'world');
setItem('foo', 'bar');
```

Since this is IDB-backed, you can store anything structured-clonable (numbers, arrays, objects, dates, blobs etc).

All methods return promises:

```js
import { setItem } from 'localstorage-idb-keyval';

setItem('hello', 'world')
  .then(() => console.log('It worked!'))
  .catch(err => console.log('It failed!', err));
```

### getItem:

```js
import { getItem } from 'localstorage-idb-keyval';

// logs: "world"
getItem('hello').then(val => console.log(val));
```

If there is no 'hello' key, then `val` will be `undefined`.

### keys:

```js
import { keys } from 'localstorage-idb-keyval';

// logs: ["hello", "foo"]
keys().then(keys => console.log(keys));
```

### removeItem:

```js
import { removeItem } from 'localstorage-idb-keyval';

removeItem('hello');
```

### clear:

```js
import { clear } from 'localstorage-idb-keyval';

clear();
```

### Custom stores:

By default, the methods above use an IndexedDB database named `keyval-store` and an object store named `keyval`. You can create your own store, and pass it as an additional parameter to any of the above methods:

```js
import { Store, setItem } from 'localstorage-idb-keyval';

const customStore = new Store('custom-db-name', 'custom-store-name');
setItem('foo', 'bar', customStore);
```

That's it!

## Installing

### Via npm + webpack/rollup

```sh
npm install localstorage-idb-keyval
```

Now you can require/import `localstorage-idb-keyval`:

```js
import { getItem, setItem } from 'localstorage-idb-keyval';
```

### Via `<script>`

* `dist/localstorage-idb-keyval.mjs` is a valid JS module.
* `dist/localstorage-idb-keyval-iife.js` can be used in browsers that don't support modules. `idbKeyval` is created as a global.
