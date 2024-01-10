---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
  - draft
source: https://javascript.info/modules-intro
---
Modules allow code to be split and organized into separate files. A module is a JavaScript file.  
the `export` and `import` directives are used to expose and load functionality between modules.

values to be exported are annotated with the `export` keyword

```javascript
// fib.js
export function fibonacci(n){
  if(n === 0 ||  n === 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2)
}
```

`import` searches for the specified path relative to the current file. Once the file is found the export object is searched for `fibonacci` and value is assigned and made available in the importing module.

```javascript
// index.js
import { fibonacci } from "./fib.js"

console.log(fibonacci(10)); // 55
```

`type=module` must be used when importing JS files that use `export`/`import` directive

```html
<script src="./index.js" type="module">
```


modules can also be imported directly in the `script` tag of a HTML document. 

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="./index.css">
  <title>Document</title>
</head>
<body>
  <h1>Hello World</h1>
  <script type="module">
    import { fibonacci } from "./fib.js"
  </script>
</body>
</html>
```

note:
  modules are only supported when using HTTP(s) protocol 
note:
  if the `src` attribute is defined the contents of  `script` are ignored 

***core module features***

modules are always in strict-mode

each module has its own top-level scope. declaration in the top-level scope of a module are not accessible other modules unless explicitly exported and imported.  
this also applies to script tag modules as well

```html
<!-- foo.js and bar.js can not access each other top level declarations -->
<!-- they would have to use explict imports and exports -->
<script src="./foo.js" type="module"></script>
<script src="./bar.js" type="module"></script>
```

```html
<script>
  const a = "hello";
</script>

<script>
  const b = "world";
</script>

<script>
  const c = a + " " + b;
</script>

<script>
  console.log(c); // "hello world"
</script>
```

```html
<!-- ReferenceError: not defined -->
  <script type="module">
    const a = "hello";
  </script>
  
  <script type="module">
    const b = "world";
  </script>
  
  <script type="module">
    const c = a + " " + b;
  </script>

  <script type="module">
    console.log(c)
  </script>
```

note:
  it is possible to defined global variables even between modules by setting them as properties to `window` .
  big no no though

```html
  <script type="module">
    window.a = "hello";
  </script>

  <script type="module">
    window.b = "world";
  </script>

  <script type="module">
    window.c = a + " " + b;
  </script>

  <script type="module">
    console.log(c); // hello world
  </script>
```


module code is only evaluated the first time the module is imported.  
once evaluated all exports are stored on an export object (maybe idk makes sense though).
all future importers of the module simply obtain the import values from the export object.

this means side-effects are run only once even if a module is imported twice either by the same module or by two different modules

```javascript
// modC.js
console.log("hello from module c");
export const test = {};
```

```javascript
// modA.js
import { test } from  "./modC.js";

console.log("modA:", test);
test.modA = "modded by module A";
```

```javascript
// modB.js
import { test } from  "./modC.js";

console.log("modB:", test);
test.modB = "modded by module B";
```

```javascript
// index.js
import { test } from "./modC.js";
import "./modA.js"
import "./modB.js"

// hello from module c

console.log(test); // { modA: "modded by module A", modB: "modded by module B" }
```

only one `hello from module c` is logged to the console.
commenting out `import "./modA.js"` removes the `modA` property from `test`
commenting out `import "./modB.js"` removes the `modB` property from `test`

this is interesting as it seemingly is side-effects on steroids, taking impurity and bringing it into the realm of whole-ass modules. Detailed in the article is a technique to leverage this behavior by having different modules responsible for the creation and initiation of an object. However given the teachings of function programming this sounds extremely error prone and a massive source of really annoying bugs

`import.meta`
an object stores some information about the current module namely the `url`

`this` in modules is undefined (modules are always in strict-mode)

modules are deferred by default `<script type="module" defer>`
- they do not block the loading of HTML, modules are loaded in parallel with other resources
- they are only executed once the HTML document is completely loaded, even if they load first
- they are always executed in the order in which they appear in the document.

this means that modules always have access to the full document regardless of where in the document they are.  This is in contrast to regular script tags which run as soon as they are encountered in the DOM.

note that the non-module `script` runs first and the module runs last

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="./index.css">
  <title>Document</title>
</head>
<body>
  <h1>Hello World</h1>
  <hr>
  <script type="module">
    console.log(btn.innerHTML); // hello world
  </script>
  
  <script>
    try { 
      console.log(btn?.innerHTML); // undefined
    } catch {
      console.log("no btn");
    }
  </script>

  <button id="btn">hello world</button>
  
  <!-- <script src="./index.js" type="module"></script> -->
</body>
</html>
```

tip:
  use loaders to prevent users form interacting with the page until deferred script are loaded and have a chance to run

`async` attributes work on inline script with `type=module` as well as for external scripts. This is in contrast to non-module scripts for whom `async` can only be used with external scripts (i.e. those with a valid `src` attribute)

`async` scripts run immediately when ready independent of other resources

the code below exhibits unusual behavior
when the non-module script tag is removed the module script tag has access to the button.
but when the non-module script tag is present the module script tag errors

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="./index.css">
  <title>Document</title>
</head>
<body>
  <h1>Hello World</h1>
  <hr>
  <script type="module" async>
    try {
      console.log(btn.innerHTML);
    } catch {
      console.log("module: no btn");
    }
  </script>

  <script>
    try { 
      console.log(btn?.innerHTML);
    } catch {
      console.log("script: no btn");
    }
  </script>

  <button id="btn">hello world</button>
  
  <!-- <script src="./index.js" type="module"></script> -->
</body>
</html>
```


External Scripts

external scripts with `type="module"` and the same `src` are run only once even when included in the html multiple times.
note: including regular scripts in html n times runs them n times

external scripts fetch from another origin require CORs headers to work.

no bare imports are allowed. the path specified to `import` must either be relative or absolute

`nomodule` attribute can be used on `script` tags to execute JS code only if a browser does not support modules 

read later
babel replacement [https://swc.rs/]