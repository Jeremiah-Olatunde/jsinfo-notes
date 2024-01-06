---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
  - draft
source: https://javascript.info/callbacks
---
asynchronous functions feature heavily in JavaScript. `setTimeout`

`loadScript` appends `script` to `document`.  
the browser loads `script` in the background asynchronously as soon as it is appended.  Once loaded the script is executed, making all its global variables available.  

```javascript
function loadScript(src) {
  let script = document.createElement('script');
  script.src = src;
  document.head.append(script);
}
```

The asynchronous loading of the script does not impede execution of the main code.
Code immediately following the `loadScript` call runs which the browser loads the script in the background.

```javascript
loadScript("script0.js"); // loading script0 in background

// code below is run immediately 
// attempting to use function in script0 before it is loaded
console.log(script0()); 

// at the end of the current script script0 runs
// "hello from script0"
```

callbacks are used to solve this problem.  
code intended to be run only after `script` has loaded are placed in a callback.  
when `script` has loaded it emits an event called `load` which can be listened to.  
`loadScript` listens for this event on `script` and run the callback once it is fired.  

```javascript
function loadScript(src, callback){
  const script = document.createElement("script");
  script.src = src;
  script.onload = callback;
  document.head.append(script);
}


loadScript("script0.js", () => {
  // hello from script0
  // run only after script0 has loaded
  // script0 contents are available here
  console.log(script0()); // I am script0
});
```

if multiple scripts are to be loaded an code executed only after all scripts are run multiple nested callback functions can be used.

```javascript
loadScript("script0.js", () => {
  // log => hello from script0
  // script0 loaded now load script1
  loadScript("script1.js", () => {
    // log => hello from script1
    // script1 loaded now load script2
    loadScript("script2.js", () => {
      // log => hello from script2
      // script1 loaded now run code
      console.log(script0(), "and",  script1(), "and", script2());
      // I am script0 and I am script1 and I am script2
    })
  })
});
```

`loadScript` can be modified to delegate the handling errors to the callback.
it passes the error (or `null` of none) as the first argument to the callback.
the second argument can be the script so the callback may make use of it

```javascript

function loadScript(src, callback){
  const script = document.createElement("script");
  script.src = src;
  script.onload = () => callback(null, script);
  script.onerror = () => callback(new Error(`loading failed: ${src}`), script);
  document.head.append(script);
}


loadScript("script0.js", (err, script) => {
  if(err) {
    console.log(err.message);
  } else {
    loadScript("script1.js", (err, script) => {
      if(err){
        console.log(err.message)
      } else {
        loadScript("script2.js", (err, script) => {
          if(err){
            console.log(err.message);
          } else {
            console.log(script0() + script1() + script2())
          }
        });
      }
    })
  }
});
```

***pyramid of doom***
nesting callbacks in this manner leads to difficult to read and maintain code.
this problems is referred to as the pyramid of doom.