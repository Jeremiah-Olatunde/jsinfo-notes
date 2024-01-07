---
source: https://javascript.info/promise-chaining
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
  - draft
---

consumers `then`, `catch`, `finally` all return promises.
this allows more consumers to be called on the returned promise
this pattern is called promise chaining


***mechanics of chaining***

***then***
`then(fulfilled, rejected)`
`fulfilled` is passed the `result` of `resolve(result)` of the promise `then` is called on
if `fulfilled` returns a promise then the `then` call returns that promise
if it returns a value that value is wrapped in a promise that resolves that value

```javascript
const promise = new Promise(resolve => resolve(42));

// these two lines are effectively the same
promise.then(result => result * 2);
promise.then(result => new Promise(resolve => resolve(result * 2)));
```

`fulfilled` probably has a default implementation which simply returns a new promise which resolves the value passed in.

```javascript
const fulfilled = value => new Promise(resolve => resolve(value));
```

this allows for values to be passed through multiple chained `then` calls until one that specifies a `fulfilled` handler is encountered

```javascript
const promise = new Promise(resolve => resolve(42));

// then calls with on rejected handlers all "skipped"
promise
  .then(null, ({message}) => console.log(message))
  .then(null, ({message}) => console.log(message))
  .then(null, ({message}) => console.log(message))
  .then(null, ({message}) => console.log(message))
  // reaches a then call that has a fulfilled handler
  .then(console.log) // 42;
```

`rejected` is passed the `error` from `reject(error)` of the promise `then` is called on
`rejected` is similar to `fulfilled` in that
if `rejected` returns a promise then the `then` call returns that promise
if it returns a value that value is wrapped in a promise that resolves that value

```javascript
const promise = new Promise((_, reject) => reject(new Error("yo mama")));

promise
  .then(console.log, ({message}) => message)
  .then(message => console.log("converted an error to result:", message))

// is identical too
promise
  .then(console.log, ({message}) => new Promise(resolve => resolve(message)))
  .then(message => console.log("converted an error to result:", message))
```

`rejected` default implementation is also similar but differ only in that it returns a promise the rejects with the passed error allowing errors to be passed down the promise chain until a `then` call with a `rejected` handler is encountered (or a `catch`)

```javascript
const rejected = value => new Promise((_, reject) => reject(error));
```

```javascript
const promise = new Promise((_, reject) => reject(new Error("yo mama")));

// then calls with only fulfilled handlers all "skipped"
promise
  .then(console.log)
  .then(console.log)
  .then(console.log)
  .then(console.log)
  // reaches a then call with a rejected handler
  .then(null, ({name, message}) => console.log(name, "=>", message));
  // Error => yo mama
```


***catch***
`catch(rejected)`
shorthand for `then(null, rejected)` so all of the above applies

note
due to the nature of the `rejected` callback if an explicit `reject` handler is specified (and the default implementation isn't used) and it doesn't return a promise that rejects with the error passed into it then the error *disappears* in a sense.
because the `rejected` callback wraps all non-promise values in a resolving promise
this essentially stops the propagation of the error down the promise chain

```javascript
promise
  .catch(null) // use default propagates error
  .then(null, null) // use default propagates error
  .catch(error => error) // convert error to resolving promise result
  .then(({name, message}) => { // handle result
    console.log("converted an error to result =>", name, message)
  })
```

***finally***
`finally(regardless)`
`regardless` callback runs `regardless` of the promise settled state.  
`regardless` (like a female heir in a conservative culture) is passed nothing.  
it does not receive any arguments (no value or error) and hence can not propagate errors or values 
moreover the values it returns are tossed away
both explicit promise returns non-promise returns (no wrapping)
all `regardless` does, is run.
`finally` itself returns the promise on which it was called

```javascript
const promise = new Promise(resolve => resolve(42));
promise
  // return non-promise value
  .finally(() => 2) 
  // return explicit Promise
  .finally(() => new Promise(resolve => resolve("hello")))
  .then(console.log); // 42 finallys had no effect
```

`finally` has no effect on the chain whatsoever  ***or does it???...***  
if  `regardless` returns a rejecting promise then *that* is returned by `finally`
if an error occurs in `regardless` then it is wrapped in a rejecting promise and returned by `finally`

```javascript
const promise = new Promise(resolve => resolve(42));


promise
  // return non-promise value (no effect)
  .finally(() => 2) 
  // return explicit Promise (no effect)
  .finally(() => new Promise(resolve => resolve("hello")))
  // return rejecting promise
  .finally(() => new Promise((_, reject) => reject(new Error("boom"))))
  // effect
  .catch(({message}) => console.log(message)); // boom
```

note that rejecting promises in `finally` hijack the errors being propagated down the promise chain

```javascript
const promise = new Promise((_, reject) => {
  reject(new Error("innocent plain on 2001-09-11"))
});

promise
  .finally(() => new Promise((_, reject) => reject(new Error("hijacked"))))
  .catch(({message}) => console.log(message)); // hijacked
```



***asides***
note the difference between promise chaining and adding multiple consumers to a single promise.

note the interesting execution order of promise consumers 
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

promise chaining can be used to neatly solve the problems sequentially performing asynchronous tasks

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

this is almost like conditionally iterating over promise calls

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