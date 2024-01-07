---
source: https://javascript.info/promise-api
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
---
there ate 6 static methods of the Promise class

```javascript
console.log(...Object.keys(Object.getOwnPropertyDescriptors(Promise)));
// all
// allSettled
// any
// race
// resolve
// reject
```

`Promise.all(iterable)`
takes an iterable of promises.
returns a promise that collects all the resolved values of every promise in `iterable` into an array and then resolves that array.
if `iterable` contains non-promise values the are wrapped in a promise that resolves the value
the order of results in the resolve results array corresponding to the order of the promises.
`Promise.all` waits till all promises in `iterable` has resolved before resolving itself
if any of the promises in `iterable` reject the promise returned by `Promise.all` rejects with the corresponding error
all other promises are ignored whether they reject or settle


```javascript
const start = Date.now();
const promise = Promise.all([
  new Promise(resolve => setTimeout(resolve, 1000, ~~(Math.random() * 10))),
  new Promise(resolve => setTimeout(resolve, 2000, ~~(Math.random() * 10))),
  new Promise(resolve => setTimeout(resolve, 3000, ~~(Math.random() * 10)))
]);

// promise takes 3000 to resolve
// waits for the last promise to resolve
promise
  .then(results => {
    console.log(results, Date.now() - start);
    // [2, 3, 6] 3000 (approximately)
  });
```


```javascript
const start = Date.now();
const promise = Promise.all([
  new Promise((resolve, reject) => {
    setTimeout(Math.random() < 0.5 ? resolve : reject, 1000, 1)
  }),
  new Promise((resolve, reject) => {
    setTimeout(Math.random() < 0.5 ? resolve : reject, 2000, 2)
  }),
  new Promise((resolve, reject) => {
    setTimeout(Math.random() < 0.5 ? resolve : reject, 3000, 3)
  })
]);

promise
  .then(results => {
    console.log(results, Date.now() - start);
  })
  .catch(error => {
    console.log(
      "rejected in:", error, "second(s) | time:", Date.now() - start
    )
  })
```


```javascript
const promise = Promise.all([0, 1, 2, 3, 4]);
promise.then(results => console.log(results)); //[0, 1, 2, 3, 4]
```

`Promise.allSettled`
similar to `Promise.all` but waits for all promises to settle (not just resolve).
returns an array with 
- `{status: "fulfilled", value: result}` for fulfilled promises
- `{status: "rejected", reason: error}` for fulfilled promises

```javascript

const start = Date.now();
const promise = Promise.allSettled([
  new Promise((resolve, reject) => {
    setTimeout(Math.random() < 0.5 ? resolve : reject, 500, 1)
  }),
  new Promise((resolve, reject) => {
    setTimeout(Math.random() < 0.5 ? resolve : reject, 1000, 2)
  }),
  new Promise((resolve, reject) => {
    setTimeout(Math.random() < 0.5 ? resolve : reject, 1500, 3)
  }),
  10, 
  20,
  30
]);

promise
  .then(results => {

    // separate the fulfilled vs rejected
    const resolved = results.filter(x => x.status === "fulfilled");
    const rejected = results.filter(x => x.status === "rejected");

    // map to extra value or reson  
    console.log(resolved.map(({value}) => value));
    console.log(rejected.map(({reason}) => reason));
  })
```


`Promise.race(iterable)`
waits for the first settled promised and gets the result or error

```javascript
const start = Date.now();
const promise = Promise.race([
  new Promise((resolve, reject) => {
    setTimeout(Math.random() < 0.5 ? resolve : reject, 1000, 1)
  }),
  new Promise((resolve, reject) => {
    setTimeout(Math.random() < 0.5 ? resolve : reject, 2000, 2)
  }),
  new Promise((resolve, reject) => {
    setTimeout(Math.random() < 0.5 ? resolve : reject, 3000, 3)
  })
]);

promise
  // always going to be approx 1 second
  .then(result => {
    console.log(
      "resolved in:", result, "second(s) | time:", Date.now() - start
    )
  })
  .catch(error => {
    console.log(
      "rejected in:", error, "second(s) | time:", Date.now() - start
    )
  })
```

`Promise.any`
waits for the first ***fulfilled*** promise (as opposed to the first ***settled*** like `Promise.race`)

if all promises reject then the promise returned from `Promise.any` rejects with a special `AggregateError` object that stores all errors in its `errors` property

`Promise.resolve(value)`
wraps `error` in a promise that resolves value

```javascript
const f = value => new Promise(resolve => resolve(value));

// both are equivalent
f(42);
Promise.resolve(42);
```

`Promise.reject(error)`
wraps `error` in a promise that resolves value

```javascript
const f = error => new Promise((_, reject) => reject(error));

// both are equivalent
f(new Error(42));
Promise.reject(42);
```