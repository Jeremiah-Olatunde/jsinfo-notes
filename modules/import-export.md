---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/import-export
---
`import` and `export` directives have several syntax variants.

***`export` before declarations***

this makes the exported value a property on the *export object*

```javascript
// foo.js
export function fibonacci(n){
  if(n === 0 ||  n === 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2)
}

export const PI = 22 / 7

export class Person {}
export class Athelete extends Person {}
```

`export {}`

```javascript
// foo.js
function fibonacci(n){
  if(n === 0 ||  n === 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2)
}

const PI = 22 / 7

class Person {}
class Athelete extends Person {}

export { fibonacci, PI, Person, Athelete }
```

`export...as`

values can be exported under a different name using the `export as` syntax

```javascript
export { fibonacci as fib, PI as delicious, Person, Athelete }
```

`import` all

`import * as namespace from "./file.js"` can be used to import all properties on the export object of `file.js` into an an object named `namespace` 

```javascript
import * as foo from "./foo.js"

console.log(foo.fib(10)); // 55

const area = r => foo.PI * r * r;
console.log(area(2)); //
```

note that `foo` is actually a *very plain object* as discussed in the prototype-methods note.

```javascript
console.log(foo.__proto__); // undefined
console.log(Object.getPrototypeOf(foo)); // null
```

the only property `foo` has aside from the imported values is `[Symbol.toStringTag]: "Module"`

this is the perfect example of a use case for very plain object.
being used as a name space

`import...as`

values imported can be reassigned to different variable names within the imported file

`import { originalName as newName } from "./file.js"`

```javascript
import { fibonacci as fib, PI as delicious } from "./foo.js"

console.log(fib(10)); // 55
console.log(delicious); // 3.142...
```


`export default`

modules can export a single *default* value using the `export default` syntax

```javascript
export default class Person {}
```

this default value can then be imported using a slightly different syntax from named exports.

```javascript
import Alias from "./foo.js"
```

note the lack of braces. 
Also note that any alias can be used and the value will be assigned to that alias 
it isn't mandatory to use the original alias the value was exported under  
in fact when using `export default` the value can be exported directly without assigned to a variable (what does this do to the context aware naming of classes and functions?)

while there can only be one default export, default exports and named exports can be combined in one module.

`default` keyword can be used to reference the default export

when exporting it can be used to export an already defined as the default export

```javascript
const PI = 22 / 7
export { PI as default }
```

it is also used when importing to import both default and named exports

```javascript 
import { Alias as default, PI, fibonacci as fib } from "./file.js"
```

when importing all exported values from a file into a name space as discussed earlier the default export can be accessed by `namespace.default`

```javascript
import * as foo from "./foo.js"

console.log(foo.default)
```

`export...from...`

allows values to be imported from a module and immediately exported.
these values are not made available in the reexporting module

```javascript
export { fibonacci as fib, PI } from "./foo.js"
```

reexporting under a name space

```javascript
// modB.js
export * as foo from "./modC.js";
```

```javascript
import * as bar from "./modB.js"

console.log(bar.foo); // object containing modC exports
```

the default export of a module can be reexported as a named export of a reexporting module

```javascript
export { test, default as Person } from "./modC.js"

export default class Animal {}
```

or the default export of one module can be reexport as the default export of the other

```javascript
export { default } from "./file.js"
```

