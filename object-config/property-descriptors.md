---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
  - draft
source: https://javascript.info/property-descriptors
banner: "![[RDT_20211231_0542298622032034228695379.jpg]]"
---
object properties have four special attributes (*flags*). 
1. `value` 
2. `writable` 
3. `enumerable`
4. `configurable`
Every object property has an associated descriptor object that stores these attributes 

`Object.getOwnPropertyDescriptor(obj, propName)` returns the descriptor of `propName` property in `obj`

`Object.defineProperty(obj, propName, descriptor)` is used to modify the descriptor object of a `propName`.
returns the object.
If the property doesn't exist then it is created.
if it does exist the attributes are updated.
if `value` attribute is unspecified it is set to `undefined`
the others are set to `false` if unspecified
(properties defined the conventional way have all attributes set to `true`)
any non-standard attributes specified in the descriptor object are discarded.

non-writable
setting `writable` to `false` makes the property non-writable i.e. the value can not be changed
an error is thrown if the value is written to (only in strict mode)
in non-strict mode the modification is silently ignored 

```javascript
// "use strict"

const person = {
  name: "Jesuseun Jeremiah Olatunde",
}

Object.defineProperty(person, "age", {
  value: 22,
  writable: false,
  enumerable: true,
  configurable: true,
})

person.age = 23; // Error in strict mode | Ignored in non strict mode
```

note:
	the descriptor can be over-written and then the value changed

```javascript
console.log(person.age); // 22
Object.defineProperty(person, "age", {writable: true});
person.age += 1;
console.log(person.age); // 23
```

non-enumerable
properties with `enumerable` set to `false` don't show up in 
- `for..in` loops
- `Object.keys`
- `console.log`
such as `toString` property of objects
tip:
	use `Object.getOwnPropertyDescriptors(obj)` to access `enumerable: false` properties

non-configurable
`configurabl: false` prevents all modifications of property attributes 
`configurable: false` also prevents property deletions
note:
	`value` can still be modified if `writable: true`
	`writable` can be changed from `true` to `false` while `configurable: false`
	not vice-versa
	
warning:
	once `configurable: false` the property descriptor can not be changed even with `defineProperty`
	
example:
	`Math.PI` has `configurable: false`. 


`Object.defineProperties(obj, descriptors)` defines property descriptors for multiple properties at once

`Object.getOwnPropertyDescriptors(obj)` returns an object containing all property descriptors

```javascript
const person = {}

const rtn = Object.defineProperties(person, {
  age: {
    value: 22,
    writable: true,
    enumerable: true,
    configurable: false,
  },
  
  name: {
    value: "Jesuseun Jeremiah Olatunde",
  }
})

console.log(Object.getOwnPropertyDescriptors(person));
```

flag aware cloning
`Object.getOwnPropertyDescriptors` and `Object.defineProperties` can be used to clone an object in a flag aware manner.
regular cloning sets the values of the copied attributes to `true` (default behaviour)
regular cloning using `for..in` loops skip `enumerable: false` properties
note:
	`Object.getOwnPropertyDescriptors` returns all symbolic and non-symbolic properties
	attempting to access the symbolic properties from the return descriptor using `Object.keys` fails
	  

```javascript
const other = Object.defineProperties({}, Object.getOwnPropertyDescriptors(person));
```

Sealing object globally

`Object.preventExtensions(obj)` 
`Object.isExensible(obj)` 

`Object.seal(obj)` 
`Object.isSealed(obj)` 

`Object.freeze(obj)` 
`Object.isFrozen(obj)` 
