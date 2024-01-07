---
source: https://javascript.info/promise-chaining
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
  - draft
---
`then` returns a promise either implicitly or explicitly 
if a promise is returned from the handler function of `then` then it becomes the result of the `then` call
if a promise is not returned from the handler function the returned value is automatically wrapped in a promise that resolves the returned value.

```javascript
const promise = new Promise(resolve => resolve(42));

// these two lines are effectively the same
promise.then(result => result * 2);
promise.then(result => new Promise(resolve => resolve(result * 2)));
```

in either case the result of the `then` call is a new promise.
as such consumers `then` can be used on this new promise to process the result.  
this is called promise chaining.

```javascript
const promise = new Promise(resolve => resolve(2));

// using explicit promise returns
promise
  .then(result => new Promise(resolve => resolve(result * 2))) // 4
  .then(result => new Promise(resolve => resolve(result * 2))) // 8
  .then(result => new Promise(resolve => resolve(result * 2))) // 16
  .then(console.log); // logs 16

// using implicit promise returns
promise
  .then(result => result * 2) // 4
  .then(result => result * 2) // 8
  .then(result => result * 2) // 16
  .then(console.log); // logs 16
```

each `then` call returns a new promise that the following `then` call is attached to.
the resolved value from the new promise is then passed to the following `then`.

note:
  the difference between promise chaining and adding multiple consumers to a single promise.
 `promise` has two handlers attached to it. each then having their own promise chain

note the interesting execution order.  
promise handlers are run after the script

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

simply returning a non promise value for it to be implicitly wrapped is useful when using the promise chain as a pseudo synchronous pipeline e.g. the code above

however if multiple asynchronous call jobs are to be chained, each running only after the other has finished then an explicit promise return is used

e.g. using promise chaining to create a make shift clock.
each `then` handler returns a new promise that waits 1 second then then resolves with the new time.

```javascript
const promise = new Promise(resolve => setTimeout(resolve, 1000, 1));

promise
  .then(result => {
    console.log(result);
    return new Promise(resolve => setTimeout(resolve, 1000, result + 1));
  })
  .then(result => {
    console.log(result);
    return new Promise(resolve => setTimeout(resolve, 1000, result + 1));
  })
  .then(result => {
    console.log(result);
    return new Promise(resolve => setTimeout(resolve, 1000, result + 1));
  })
  .then(result => {
    console.log(result);
    return new Promise(resolve => setTimeout(resolve, 1000, result + 1));
  })
  .then(result => {
    console.log(result);
    return new Promise(resolve => setTimeout(resolve, 1000, result + 1));
  })
```

turning the above code into a function that takes the time as an argument leads to interesting problems. 
using recursion to optionally add `.then` call to the promise chain depending on the result of the previous promise.
each call to `f` adds a `then` to the passed promise
recursively call `f` with a new promise to continue the chain

```javascript
function countTo(seconds){
  const start = new Promise(resolve => setTimeout(resolve, 1000, 1));
  
  const f = clock => {
    clock.then(time => {
      console.log(time);
      if(time < seconds) {
        f(new Promise(resolve => setTimeout(resolve, 1000, time + 1)))
      } else {
        console.log("done:", time)
      }
    })
  }

  f(start);
}

countTo(10);
```

promise handlers are executed after the currently executing script is finished


***load script***
the benefit of promises can be seen when implementing `loadScript` using chaining
`loadScript` returns a promise that resolves when the script is loaded
`then` handler is attached and it can call `loadScript` again and return its return value (which again is a new promise)
this can be repeated until all the desire scripts are loaded sequentially
the final `then` can then run code to be executed only after all scripts are loaded

note how the code grows down in a readable manner in contrast to the pyramid of doom

```javascript
function loadScript(src){
  return new Promise((resolve, reject) => {
    const script = document.createElement("script");
    script.src = src;
    script.onload = resolve; // resolve called with event
    script.onerror = reject; // reject called with error
    document.head.append(script);
  });
}

loadScript("script0.js")
  .then(() => loadScript("script1.js"))
  .then(() => loadScript("script2.js"))
  .then(() => console.log(`${script0()} and ${script1()} and ${script2()}`));
```

note:
promise handlers may also return *thenable* object.
a *thenable* is an object that has a `then(resolve, reject)` method.
if a handler returns a thenable the `then` method is invoked with `resolve` and `reject`

```javascript

const thenable = {
  curr: 0,
  then(resolve, reject){
    // thenable.curr as this is lost and binding is too verbose
    setTimeout(resolve, 1000, thenable.curr++)
  }
}


const promise = new Promise(thenable.then);

promise
  .then(result => {
    console.log(result); // 0
    return thenable;
  })
  .then(result => {
    console.log(result); // 1
    return thenable;
  })
  .then(result => {
    console.log(result); // 2
    return thenable;
  })
  .then(result => {
    console.log(result); // 3
    return thenable;
  })
  .then(console.log);
```