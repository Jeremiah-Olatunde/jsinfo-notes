---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/prototype-methods
---
`__proto__` is depreciated 

`Object.setPrototypeOf(Obj, proto)` and `Object.getPrototypeOf(obj)` are modern methods for accessing the `[[Prototype]]` of an object

using `__proto__` when creating on object using the object literal syntax is not considered bad practice

`Object.create(proto[, descriptors])` can also be used instead
created a new empty object and sets its `[[Prototype]]` to `proto` and optional descriptors

`Object.create`, `Object.getOwnPropertyDescriptors` and `Object.getProptotypeOf` can be used in conjunction to create a more accurate shallow copy of an object
copies properties, their descriptor attributes and also copies the prototype

`Object.define` and `Object.getOwnPropertyDescriptors` method does not copy the prototype

warning:
	changing `[[Prototype]]` of an object has serious performance impacts


very plain objects


`__proto__` is a getter/setter for the `[[Prototype]]` of an object.
it is inherited 
attempting to set `[[Prototype]]` to a none object using `__proto__`  fails silently
`__proto__` has validation logic to ensure the value is an object or `null`


`__proto__` is an accessor property.
hence when writing the prototype chain is searched
(writing to data properties ignores the prototype chain and writes on the object itself)

```javascript
const foo = Object.create(Object.prototype);

foo["__proto__"] = 42; // calls the setter of __proto__ which ignores the primitive

console.log(foo.__proto__);// still Object.prototype 
```

if an object is intended to be used as only an associative array or a dictionary this creates a problem
`__proto__` can not be used reliably

setting the `[[Prototype]]` of an object to null removes the prototype chain completely
object looses access to `__proto__` accessor property
hence reading and writing to `__proto__` works like a normal data property on the object itself

```javascript
const foo = Object.create(null);

foo["__proto__"] = 42

console.log(foo); // { ['__proto__']: 42 } 
```

note:
	for some reason `__proto__` is printed out in computed property notation
	
```javascript
const foo = Object.create(null);

foo.__proto__ = 42
foo.__lorem__ = 42

console.log(foo);
// { ['__proto__']: 42, __lorem__: 42 }
```


note:
	these plain objects loose all instance properties
	however the static methods on the `Object` constructor still works on them

