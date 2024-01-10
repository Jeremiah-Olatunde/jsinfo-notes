---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/new-function
banner: "![[RDT_20211230_2339317963621244823273346.jpg]]"
---
the new function syntax is used to create functions from strings dynamically at run time.
the last argument is a string representing the code body of the function
the arguments before the last (if any) are the parameters of the function. 
the parameters must be a valid JavaScript parameter (plain identifier, rest, destructed, default)
```javascript
const sayHi = new Function("a", "b", "return a + b;")
```

this is typically used in specific cases e.g. receiving a code from a server to dynamically compile a function from a template

closure
functions created with the Function constructor syntax have their hidden internal `[[Environment]]` property set to reference the *global scope*.
as such they only have access to their local scope and the global scope, irrespective of definition location

read later
[Function Constructor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/Function)