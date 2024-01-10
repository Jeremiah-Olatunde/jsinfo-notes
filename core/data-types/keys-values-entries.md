---
tags:
  - backend
  - draft
  - frontend
  - jsinfo
  - javascript
source: https://javascript.info/keys-values-entries
---
`keys`, `values` and `entries` methods are intended to be generic 
it is best practice to implement them on custom data structures 
`Map`, `Set` and `Array` implement them

plain objects also implement them but as static methods on the `Object` class
`Object.keys(obj)`
`Object.values(obj)`
`Object.entries(obj)`
note:
	these methods ignore symbol properties
	`Object.getOwnPropertySymbols` and `Reflect.ownKeys` return symbols

this is done to avoid cluttering the method namespace of custom objects leaving objects as flexible as possible
`Object` static methods all return real array object not just iterables like `keys`, `values` e.t.c.

tip:
	to use array methods (`map`, `filter`) on objects 
	convert the object to an iterable using `Object.entries` (remember returns real array) 
	call desired method
	convert back to object `Object.fromEntries`
	note `Map`, `Set` and `Array` entries do not return real array, there return and iterable/iterator
	no `map`, `filter` properties on them
	use `Array.from`
