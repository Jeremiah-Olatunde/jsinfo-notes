---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/prototype-inheritance
---
object have a special hidden property called `[[Prototype]]`
the value of this property is either another object or `null`
when a property is *read* from an object, the prototype chain is searched starting from the object until the property is found. 
this mechanism is call prototypal inheritance
properties defined on an object in the prototype chain of another object are called inherited properties 

`__proto__` is an accessor property (getter and setter) for the hidden `[[Prototype]]` property


```javascript
const person = { name: "JJO" };
const athelete = { sport: "football", };
athelete["__proto__"] = person;
athelete["__proto__"] = 42; // primitive is ignored


console.log(athelete.__proto__); // person
console.log(athelete.name); // JJO
```

warning:
	circular prototype chains are not allowed. an error will be thrown
	`__proto__` can only be `null` or another object if a primitive value is set it is ignored

warning: 
	if `__proto__ === null` accessing `__proto__` returns undefined.
	using `Object.getPrototypeOf` returns `null` as expected.


```javascript
const person = { name: "JJO" };
const athelete = { sport: "football" };

athelete["__proto__"] = person;
person["__proto__"] = athelete;

// TypeError: Cyclic __proto__ value
```

note:
	`Object.getOwnPropertyDescriptor` and `Object.defineProperty` are unable to manipulate the `__proto__` property.
	`Object.getOwnPropertyDescriptor(obj, "__proto__")` returns `undefined`


`Object.defineProperty` is extremely weird

```javascript
const person = { name: "JJO" };
const athelete = { sport: "football" };

athelete["__proto__"] = person;


Object.defineProperty(athelete, "__proto__", {
  value: null, // seemingly set prototype to null
  enumerable: true // show up in console.log
})

console.log(athelete.__proto__); // null as expected
console.log(athelete); // { sport: 'football', ['__proto__']: null } so far so good

console.log(athelete.name); // JJO does not affect prototypal inheretance, still works
console.log(Object.getPrototypeOf(athelete)); // person still
```

probably due to poor design choices made prior to `ES2015`
avoid using `__proto__` all together

writing does not use the prototype
write and delete operations work directly with the object

```javascript
const person = { name: "JJO" };
const athelete = { sport: "football" };

athelete["__proto__"] = person;


console.log(athelete.name); // JJO from person

athelete.name = "Jesuseun"; // sets name property on athelete

console.log(athelete.name); // Jesuseun
console.log(person.name); // person name property unchanged
```

accessor properties are different
write to an accessor property invokes the setter on the prototype chain as opposed to writing over the property on the original object 
assignments to accessor properties are treated as function calls not assignments 

```javascript
const person = { 
  firstName: "first", 
  lastName: "last", 
  set fullName(value){
    [this.firstName, this.lastName] = value.split(" ");
  },

  get fullName(){
    return this.firstName + " " + this.lastName;
  }
};
const athelete = { sport: "football" };

athelete["__proto__"] = person;

athelete.fullName = "Jesuseun Olatunde";
// instead of writing fullName: "Jesuseun Olatunde" on athelete
// calls fullName setter on person
// athelete.fullName("Jesuseun Olatunde"); no assignment to fullName 

console.log(athelete); 
// firstName and lastName properties added on athlete
// not fullName

```

value of this

`this` always references the object before the dot regardless of the location of a method in the prototype chain.
`this` in methods is evaluated at call time an not dependent on the method in which it was defined
ergo modifying `this` object modifies the object on which the method it was defined 

```javascript

const person = {
  name: "person",
  setName: function(value){
    this.name = value;
  }
}

const athelete = {
  __proto__: person
}

athelete.setName("athelete"); // this in setName method becomes athelete

console.log(athelete.name); // athelete;
console.log(person.name); // person
```

methods are shared but the object state is not

looping
`for..in` loops iterate over inherited properties too
`obj.hasOwnProperty(propName)` returns `true` if `propName` is not inherited and `false` otherwise.

`Object.keys` and `Object.values` both ignore inherited properties 


note:
	All methods found on `Object.prototype` are `enumerable: false`