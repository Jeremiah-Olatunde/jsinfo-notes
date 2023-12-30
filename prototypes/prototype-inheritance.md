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
properties defined on some object in the prototype chain of an object are called inherited properties 

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

note: 
	if `__proto__ === null` accessing the prototype via `__proto__` returns undefined.
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

probably due to pre-`ES2015` weirdness
avoid using `__proto__` all together

writing to an object does not use the prototype chain
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

```javascript
const person = { name: "person" };
const athelete = { __proto__: person };

delete athelete.name;
console.log(person); // { name: "person" } remains unaffected
```

accessor properties are different.
writing to an accessor property invokes the corresponding setter on the prototype chain as opposed to writing over the property on the original object.
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

note:
	if a data property is found on the prototype chain before an accessor property of the same name write and delete operations work on the object itself as per usual

```javascript
const person = { 
  name: "person", 
  get fullname(){ return this.name; },
  set fullname(value){ this.name = value + "setter"; } 
  // "setter" identifies when this setter is invoked
};

const athelete = { 
  __proto__: person,
  fullname: "yo mama" // override accessor fullname with a data fullname
};

athelete.fullname = "yo father";

console.log(athelete.fullname); // "yo father" fullname setter on person not invoked
delete athelete.fullname; // removing own fullname property

// with no fullname property on athelete the prototype chain is searched
// the accessor full name property on person is found and the setter invoked
athelete.fullname = "lorem"; 

console.log(athelete.fullname); // loremsetter 
```

```javascript
const person = { 
  name: "person", 
  get fullname(){ return this.name; },
  set fullname(value){ this.name = value + "setter"; }
};

const athelete = { 
  __proto__: person,
  fullname: "yo mama"
};

const footballer = {
  __proto__: athelete,
}

console.log(footballer.fullname); // "yo mama" from athelete

// encounters data fullname on athelete before accessor fullname on person
// hence performs wirte operation on footballer without invoking accessor
footballer.fullname = "lionel mession"; 
console.log(footballer); // has own fullname property now
delete footballer.name; // deletes own fullname property
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
descriptor methods ignore inherited properties...obviously 
`Object.getOwnPropertyDescriptors`

note:
	All methods found on `Object.prototype` are `enumerable: false`