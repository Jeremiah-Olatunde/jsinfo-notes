---
tags:
  - backend
  - frontend
  - draft
  - javascript
  - jsinfo
source: https://javascript.info/microtask-queue
---
promise consumers are always executed asynchronously even when a promise resolves immediately in its synchronous executor.
This behavior occurs due to the *mircotask* queue

```javascript
const promise = new Promise(resolve => {
  resolve(2)
  console.log("runs first");
});

promise
  .then(result => result * 2) // 4
  .then(result => result * 2) // 8
  .then(result => result * 2) // 16
  .then(result => {
    console.log("runs last");
    console.log(result);
  }); // logs 16


console.log("runs second");
```


***Microtask Queue***
The ECMA standard specifies an internal queue responsible for managing the execution of asynchronous promise code. This internal queue is officially referred to as the `PromiseJobs` and colloquially referred to as the Microtask queue

- the queue is a first in first out queue 
- tasks on the queue are only executed when nothing else is running

when a promise is settled the callbacks passed into the consumers are placed on the microtasks queue. When the engine becomes idle (i.e. completed execution of current code) then it runs tasks from the microtask queue

***THIS INFORMATION IS EXTREMELY INCOMPLETE***
covered in more detail a bit later I think
look at this shit

```javascript
const promise = new Promise((resolve) => {
  resolve(2);
  console.log("first");
});

// PART 1
promise // promise1A
  // then 1A
  .then(x => {
    console.log("bar:", x);
    return x * 2;
  }) // returns promise1B
  // then 1B
  .then(x => {
    console.log("bar:", x);
    return x * 2;
  }) // returns promise1C
  // then 1C
  .then(x => {
    console.log("bar:", x);
    return x * 2;
  }) // returns promise1D
  // then 1D
  .then(x => {
    console.log("bar:", x);
    return x * 2;
  }) // returns promise1E
  // then 1E
  .then(console.log)


// PART 2
promise // promise2A
  .then(x => { // then2A
    console.log("foo:", x);
    return x * 2;
  }) // returns promise2B
  .then(x => { // then2B
    console.log("foo:", x);
    return x * 2;
  }) // returns promise2C
  .then(x => { // then2C
    console.log("foo:", x);
    return x * 2;
  }) // returns promise2D
  .then(x => { // then2D
    console.log("foo:", x);
    return x * 2;
  }) // returns promise2E
  .then(console.log)

// first
// bar: 2
// foo: 2
// bar: 4
// foo: 4
// bar: 8
// foo: 8
// bar: 16
// foo: 16
// 32
// 32

```

possible explanation

running through the code in execution order

***part 1***
`promise1A` has a handler attached to it via `then1A`. 
this handler is stored internally in `promise1A`. (to be placed on the microtask queue on resolve)
`then1A` returns a new promise (`promise1B`).

`promise1B` has a handler attached to it via `then1B`.
this handler is stored internally in `promise1B`. (to be placed on the microtask queue on resolve)
`then1B` returns a new promise (`promise1C`).

*…and so on*

***part 2***
`promise1A` has another handler attached to it. this time via `then2A`.
this handler is stored internally in `promise1A`.

now `promise1A` has two handlers stores on it internally (from `then1A` and `then2A`)
they are stored in a FIFO queue
hence when `promise1A` resolves its handlers are put in a first come first sever manner.

`then2A` returns a new promise (`promise2B`).

from here it is business as in part 1

`promise2B` has a handler attached to it via `then2B`.
this handler is stored internally in `promise2B`. (to be placed on the microtask queue on resolve)
`then2B` returns a new promise (`promise2C`).

*…and so on*

***now we start cooking***

`MTQueue: []`

`promise1A` resolves first
`handler1A` and `hanlder2A` are placed on the microtask queue (in that order)

`MTQueue: [handler1A, handler2A]`

***block 1***

since there is no currently executing code `handler1A` is popped of the microtask queue and run

`MTQueue: handler1A <- [handler2A]`

***log***: `bar: 2`

recall that handlers that return non-promises are wrapped in promise that resolve the returned value of the handler.

`handler1A` return value is resolved by `promise1B`
hence `handler1B` is placed on the microtask queue

`MTQueue: [handler2A, handler1B]`

***block 2***

again once the engine is free `handler2A` is popped off the queue 

`MTQueue: handler2A <- [handler1B]`

***log***: `foo: 2`

`handler2A` return value is resolved by `promise2B`
hence `handler2B` is placed on the microtask queue

`MTQueue: [handler1B, handler2B]`

***block 3***

engine is free hence `handler1B` is popped of the queue and ran

`MTQueue: handler1B <- [handler2B]`

***log***: `bar: 4`

`handler1B` return value is resolved by `promise1C`
hence `handler1C` is placed on the microtask queue

`MTQueue: [handler2B, handler1C]`

***block 4***

engine is free hence `handler2B` is popped of the queue and ran

`MTQueue: handler2B <- [handler1C]`

***log***: `foo: 4`

`handler2B` return value is resolved by `promise2C`
hence `handler2C` is placed on the microtask queue

`MTQueue: [handler1C, handler2C]`

***repeat until end***

***unhandled rejection***
If all the scheduled tasks on the microtask queue are completed and a promise error has not been handled an *unhandledrejection* event is emitted.

recall that consumers can be used to schedule promise handlers even after the promise is settled

```javascript
let promise = Promise.reject(new Error("Promise Failed!"));
setTimeout(() => promise.catch(err => alert('caught')), 1000);

// Error: Promise Failed!
window.addEventListener('unhandledrejection', event => alert(event.reason));

// sometime later
// "caught"
```