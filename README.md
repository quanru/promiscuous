# promiset
<a href="http://promises-aplus.github.com/promises-spec">
  <img src="http://promises-aplus.github.com/promises-spec/assets/logo-small.png"
       alt="Promises/A+ logo" title="Promises/A+ 1.0 compliant" align="right" />
</a>

promiset is a tiny implementation of the [Promises/A+ spec](http://promises-aplus.github.com/promises-spec/).

It is promise library in JavaScript, **small** (< 1kb [minified](https://raw.github.com/RubenVerborgh/promiset/dist/promiset-node.js) / < 0.6kb gzipped) and **fast**.

## Installation and usage
### Node
First, install promiset with npm.
```bash
$ npm install promiset
```

Then, include promiset in your code file.
```javascript
var Promise = require('promiset');
```

### Browsers
Include [promiset](https://raw.github.com/quanru/promiscuous/dist/promiset-browser.js) in your HTML file.
```html
<script src="promiset-browser.js"></script>
```

This version (and a minified one) can be built with:
```bash
$ build/build.js
```

## API
### Create a resolved promise
```javascript
var promise = Promise.resolve("one");
promise.then(function (value) { console.log(value); });
/* one */
```

### Create a rejected promise
```javascript
var brokenPromise = Promise.reject(new Error("Could not keep promise."));
brokenPromise.then(null, function (error) { console.error(error.message); });
/* "Could not keep promise." */
```

You can also use the `catch` method if there is no success callback:

```javascript
brokenPromise.catch(function (error) { console.error(error.message); });
/* "Could not keep promise." */
```

### Write a function that returns a promise
```javascript
function promiseLater(something) {
  return new Promise(function (resolve, reject) {
    setTimeout(function () {
      if (something)
        resolve(something);
      else
        reject(new Error("nothing"));
    }, 1000);
  });
}
promiseLater("something").then(
  function (value) { console.log(value); },
  function (error) { console.error(error.message); });
/* something */

promiseLater(null).then(
  function (value) { console.log(value); },
  function (error) { console.error(error.message); });
/* nothing */
```

### Convert an array of promises into a promise for an array
```javascript
var promises = [promiseLater(1), promiseLater(2), promiseLater(3)];
Promise.all(promises).then(function (values) { console.log(values); });
/* [1, 2, 3] */
```

### Execute an array of promises, returns a promise that resolves or rejects as soon as one of the promises in the iterable resolves or rejects, with the value or reason from that promise.
```javascript
var promises = [promiseLater(1), promiseLater(2), promiseLater(3)];
Promise.race(promises).then(function (values) { console.log(values); });
/* 1 */
```

### Execute an array of promises serially, returns a promise with array contain the value of every resolved promise, but it will reject if any of the promises is rejected.
```javascript
var promises = [promiseLater(1), promiseLater(2), promiseLater(3)];
Promise.serial(promises).then(function (values) { console.log(values); });
/* [1, 2, 3] */
```
