---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
  - draft
source: https://javascript.info/symbol
---
`string` and `symbol` primitives are the only values that can be used as object keys
if a value of any other type it used it is [[type-conversions#String Conversions|converted]] to a string.
`symbol`s are unique identifiers.  They are unique primitive values with an optional description.
No two symbols are ever the same (unless the global symbol registry is used).
`symbol`s are created using `const sym = Symbol(description)` where `description` is a `string`.
note that two symbols created with the same description are still different entities.
warning `symbol`s do not auto-convert to `string`s. `.toString()` method must manually be called on symbol if its string form (`Symbol()` or `Symbol(description)`) is required.

The [[object#Computed properties|computed property]] syntax is used to create properties with `symbol` keys when using [[object#Object creation|object literals]]. The [[object#Square bracket syntax|square bracket]] syntax is used to add symbol properties to already existing objects.
Note that the created `symbol` must first be stored in a variable so that it can reused when accessing the property. If the variable containing the `symbol` key of a property is lost (goes out of scope or is reassigned) then that property become inaccessible...forever.

`symbol`s allow for the creation of hidden object properties.
no other code an ever create a symbol that equals the that of the hidden property's again.
this allows for the safe addition of properties to objects belonging to external code bases.
if `string` property keys were used instead the could clash and overwrite properties of the original code base thereby breaking functionality. 
Using `symbol` property keys guarantees that no existing key will be over written and that in the future the `symbol` key won't be over written as well.

properties with `symbol` keys are skipped during property iteration i.e. [[object#`for..in` loops|`for...in`]] loops and [`Object.keys`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys). [`Object.assign`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) however copies them just like with `string` keys.

As mentioned earlier if the variable containing a symbol is lost then access to that symbol and any properties for which that symbol was a key is lost along with it. To prevent this JavaScript provides a *global symbol registery*.
This allows for the creation of symbols that can then be accessed later using the symbol description.
`Symbol.for(key)` is used to create `symbol`s and store them in the global registry if they are not already present. If a `symbol` with a matching key is present then it is returned.
example with `Symbol()` and `Symbol.for()`
`Symbol.keyFor(symbol)` return the key for the `symbol` passed in if `symbol` is in the global symbol registry. If it isn't then it returns `undefined`. 

System symbols are used internal by JavaScript and are exposed to programmers as a means to fine tune object behavior. Some of them include:
- `Symbol.hasInstance`
- `Symbol.iterator`
- `Symbol.toPrimitive`

read later
[Well known symbols](https://tc39.github.io/ecma262/#sec-well-known-symbols)

