---
source: https://javascript.info/modules-dynamic-imports
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
---
modules can be imported dynamically using the `import(modulePath)` syntax  
`modulePath` is an expression that evaluated to a `string`
this returns a promise that resolves into a module object (discusses earlier) that contains all the exports of the imported module.
the promise rejects if the module was not found

```javascript
const promise = Math.random() < 0.5 ? import("./modA.js") : import("./modB.js");

promise
  .then(({ default: result }) => console.log(result))
  .catch(err => console.log(err.message));
```

note that the default export is saved in the default property of the module object.
recall JavaScript keywords can be used as object property names but not variable names
hence when using parameter destructuring it must be renamed.

dynamic imports do not require `type=module`
`import()` isn't a function call. `import` isn't a function it doesn't inherit from `Function.prototype`.
it is simply a special syntax.