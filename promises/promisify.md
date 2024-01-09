---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source:
---
promisification is the conversion of a callback based function into a promise based alternative.  

Original `loadScript` function

```javascript
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;

  script.onload = () => callback(null, script);
  script.onerror = () => callback(new Error(`Script load error for ${src}`));

  document.head.append(script);
}
```

Promisified version

```javascript
let loadScriptPromise = function(src) {
  return new Promise((resolve, reject) => {
    loadScript(src, (err, script) => {
      if (err) reject(err);
      else resolve(script);
    });
  });
};
```

Essentially `loadScript` is wrapped by function that returns a promise instead.
This function has `src` as the only parameter, hiding the callback from the calling code.
The executor of the returned promise is responsible for invoking `loadScript` providing it with a custom callback.
When `loadScript` runs and it's result its ready it calls the provided callback as normal
the custom callback takes arguments provided by `loadScript` and then calls resolve or reject.
note:
  that `resolve` and `reject` can be called from the callback due to closures 
  function have access to the environment record of the scope in which they were defined.
  the `loadScript` was defined inside the promise executor which has `resolve` and `reject` on its environment record
note:
  in `loadScriptPromise` the assumption is made about the signature of the  `loadScript` callback

the `loadScriptPromise` code can be generalized.
A decorator can be made to promisify any callback based code

```javascript

function promisify(f){
  return function wrapper(...args){
    return new Promise((resolve, reject) => {
      f.call(this, ...args, (error, results) => {
        error ? reject(error) : resolve(result)
      });
    })
  }
}
```

the decorator returns a new wrapped function that returns a promise to replace the callback based function.  
the executor of the promise calls the original callback function.
it passes in the arguments gathered by `wrapper` but also passes in as the last argument the custom callback.
as before this custom callback calls `resolve` or `reject` based on the arguments passed in to the callback by `f`
note `f.call(this, ...)` to enable it to retain context when used as a method.

note:
  again the code assumes the signature of `f`, the callback is always the last argument
  the signature of the callback of `f` is also assumed to be `f(error, result)`

the decorator can be further generalize to operates of functions which pass in multiple results into their provided callbacks by using rest parameters 

```javascript
function promisify(f){
  return function(...args){
    return new Promise((resolve, reject) => {
      f.call(this, ...args, (error, ...results) => {
        if(error) reject(error);
        else if(results.length === 1) resolve(results[0]);
        else resolve(results);
      });
    })
  }
}
```

this new decorator invokes the callback with the array of results if there are more than on results or with the single results if there is only one result