---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
  - draft
source: https://javascript.info/promise-basics
---
Promises in JavaScript were introduced to fit the pattern of problems that arise
- producing code does something that takes times
- consuming code that should be run once the producing code has been completed

syntax

```javascript
const promise = new Promise(function(resolve, reject){
  // code
});
```

the function passed into the `Promise` constructor is called the *executor* function  
it is run immediately and automatically.
two callbacks (`resolve` and `reject`) are passed as arguments to the executor.  
the executor runs some code (usually asynchronously) and calls one of the callbacks:
- `resolve` should be invoked with the result if the operation was successful
- `reject` should be invoked with the error otherwise 

the promise instance returned contains two internal properties `state` and `result`
initially these properties are `"pending"` and `"undefined"`
if `resolve(value)` is called they become `fulfilled` and `value`
if `reject(error)` is called they become `rejected` and `error`

a promise that is either `fulfilled` or `rejected` is said to be ***settled***

note that the executor function is run ***immediately*** and ***synchronously***
it does not ***have*** to implement any asynchronous logic 

```javascript
const promise = new Promise((resolve, reject) => {
  // no async code here. all code below runs before the script continues
  console.log("first");
  resolve(42);
  console.log("second");
});

console.log("third");
```

also note that `resolve` and `reject` do not force the executor function to return.  
code below a `resolve` or `reject` call will still run.  


the main utility of promises are in asynchronous code

```javascript
const promise = new Promise((resolve, reject) => {
  console.log("first");

  // scheduling some asynchrnous work
  setTimeout(() => {
    // calling resolve(result) aftet the work has been completed
    console.log("third");
    resolve(42);
  }, 2000);
});

console.log("second");
```

`resolve` and `reject` do not force the executor function to return.  
code below a `resolve` or `reject` call will still run.  

there can only be one called to `resolve` or `reject`.  
subsequent calls are ignored.  

`reject` can be called with any value but instances of `Error` are recommended


***consumers***

the returned promise instance contains a set of methods for performing actions once the executor has completed its works

`then(f, f)` takes two callbacks are arguments  
the first is run and passed the result if the promise was resolved 
the second is run and passed the error if the promise was rejected 

```javascript
promise.then(
  function(result){ /* code to run if successful*/ },
  function(error){ /* code to run if failure*/ },
)
```


```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    Math.random() < 0.5 ? resolve("you win") : reject(new Error("you loose"))
  }, 2000);
});

// prints you win or you loose after 2000ms
promise.then(
  (result) => console.log("successful:", result),
  (error) => console.log("oops:", error.message)
);
```

if only successful cases are to be handled then the second argument can be omitted `.then(f)`
if only errors are to be handles then `null` can be passed in as the first argument `.then(null, f)`

`catch(f)` can be used instead of `then(null, f)
the `catch` handler is passed the error is a promise rejects

```javascript
promise
  .catch((error) => console.log("oops:", error.message))
```

```javascript
promise
  .then(result => console.log("success:", result))
  .catch(error => console.log("oops:", error.message))
```

`finally(f)` is used to run a function regardless of whether or not the promise resolved or rejected.
typically used for some clean up code
`finally` handler takes no arguments 
`finally` also "passed through" any results or error

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    Math.random() < 0.5 ? resolve("you win") : reject(new Error("you loose"))
  }, 1000);
});

promise
  // finally run first, passes through the result or error to the next
  .finally(() => console.log("game start"))
  .then(
    // runs if promise resolve(result)
    result => console.log("success:", result),
  
    // runs if promise reject(result)
    error => console.log("oops:", error.message)
  )
  // runs regardless performs clean up
  .finally(() => console.log("game over"))
```

note:
  returned values from `finally` handler are ignored
  errors thrown in `finally` handler are passed on to subsequent handlers

multiple handlers can be attached to one promise

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    Math.random() < 0.5 ? resolve("you win") : reject(new Error("you loose"))
  }, 1000);
});

// all run when the promise resolves
promise.then(result => console.log("success 0:", result));
promise.then(result => console.log("success 1:", result));
promise.then(result => console.log("success 2:", result));


// all run when promise rejects
promise.catch(error => console.log("shame 0:", error.message));
promise.catch(error => console.log("shame 1:", error.message));
promise.catch(error => console.log("shame 2:", error.message));
```

this difference from promise chaining to be covered later

note:
  handler can be attached to promises that are already settled 
  when this happens the handlers are run immediately
  however if an error occurs and `reject` is called and there are no handlers for it a global script uncaught error is thrown.

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    Math.random() < 0.5 ? resolve("you win") : reject(new Error("you loose"))
  }, 1000);
});


setTimeout(() => {
  promise
    .finally(() => console.log("game start"))
    .then(result => console.log("success:", result))
    .catch(error => console.log("oops:", error.message))
    .finally(() => console.log("game over"))
}, 3000)
```



note:
  difference between chaining and attatching multiple handlers to a promise