---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/async-await
---
`async`/`await` provide a more convenient and concise method for working with promises

`async` functions

```javascript
async function f(){
  return 42;
}
```


`async` functions (like callbacks of `then`/`catch`/`finally`) always return a promise.
if a non-promise value is returned it is wrapped in a promise that resolves that value

```javascript
async function f(){
  return 42;
}

async function g(){
  return new Promise(resolve => resolve(42))
  // return Promise.resolve(42);
}
```

if an error is thrown inside an `async` function then it is wrapped in a rejecting promise and that promise is returned

```javascript
async function f(){
  throw new Error("42");
}

async function g(){
  return Promise.reject(new Error(42))
}
```



`await`
`await` can only be used inside of `async` functions.
it asynchronously suspends the execution of the function until the promise settles.
if the promise is fulfilled the resolved value is returned by `await`

```javascript
const result = await promise;
```


```javascript
async function f(){
  // runs synchronously initially
  console.log("first");

  // encounters await and asynchronously suspends function for 1000ms
  const result = await new Promise(r => setTimeout(r, 1000, 42));
  
  // promise resolves and f executions is resumed
  console.log("third:", result);
}

// invoke f
f();

// f is suspended 
// hence this continues
console.log("second");

// script ends engine is free
```

the `then` equivalent

```javascript
"use strict";

function f(){
  // runs synchronously cause regular function duh
  console.log("first");

  new Promise(r => setTimeout(r, 1000, 42))
    // using then to run the rest of the function 
    // only after promise resolves
    .then(result => {
      // promise results rest of f executes
      console.log("fourth:", result);
    });

  // function continues to synnchronously execute and returns
  console.log("second")
}

// invoke f
f();

// f completes and returns
// script continues as normal
console.log("third");

// script ends engine is free
// jump into then callback
```

`await` can not be used in regular functions
`await` can be used in classes and object literals to make methods asynchronous 

top level `await` (`await` outside of any function) is supported in modules

`await` accepts thenables. simply suspense executions asynchronously until the `then` method of the thenable calls the supplied `resolve`/`reject` callbacks.

***error handling***

if the promise rejects then await throws the error the promise rejected with

```javascript
async function f() {
  await Promise.reject(new Error("Whoops!"));
  // becomes
  throw new Error("Whoops!")
}
```

a `try`/`catch` block can be used to catch the error and handle it

if the error is not caught the promise returned by the `async` function rejects with the error 