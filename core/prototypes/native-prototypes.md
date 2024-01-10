---
tags:
  - backend
  - frontend
  - draft
  - javascript
  - jsinfo
source: https://javascript.info/native-prototypes
banner: "![[RDT_20211230_2341538258584312757593932.jpg]]"
---

`Object.prototype`
when a new object is created using the object literal `{}` or object constructor syntax `new Object()` its `[[Prototype]]` is set to `Object.prototype`

`Object.prototype` contains the definitions for the built in methods and properties available to objects.
When these methods are access on objects `obj.hasOwnProperty` they are got from the prototype chain

```javascript
// descriptor objects of all object instance methods
// e.g hasOwnProperty, toString, propertyIsEnumerable etc
console.log(Object.getOwnPropertyDescriptors(Object.prototype));

console.log({}.toString === Object.prototype.toString); // true
```

note:
	the `[[Prototype]]` of `Object.prototype` is `null`
	`Object.prototype.__proto__ === null`

All built in function constructors have their own prototypes as well that implement the instance methods found on instances of the constructors.

```javascript
// call, bind, apply, toString etc
console.log(Object.getOwnPropertyDescriptors(Function.prototype));

// forEach, every, map etc
console.log(Object.getOwnPropertyDescriptors(Array.prototype));


console.log(Object.getOwnPropertyDescriptors(Number.prototype));
// String, Symbol, Map, Set etc
```

when a new instance of a constructor is made (either via the function constructor itself or the corresponding literal syntax) its `[[Prototype]]` is set to the prototype property of its corresponding constructor

```javascript
console.log([].__proto__ === Array.prototype); // true
console.log((function(){}).__proto__ === Function.prototype); // true
console.log(5..__proto__ === Number.prototype); // true
console.log("".__proto__ === String.prototype); // true
console.log(new Map().__proto__ === Map.prototype); // true
console.log(new Set().__proto__ === Set.prototype); // true
```


everything is an object
arrays, functions, maps, sets etc. are all objects
they inherit the methods and properties from the `Object.prototype`
That is `Object.prototype` exists on their prototype chain (at the top).

```javascript

//      SomeConstructor.prototype    Object.prototype
//                     |                   |
// someInstance -----> * ----------------> * -------------> null

// function prototype chain
//                Function.prototype    Object.prototype
//                        |                   |
// someFunction --------> * ----------------> * -------------> null


// array prototype chain
//                Array.prototype    Object.prototype
//                     |                   |
// someArray --------> * ----------------> * -------------> null

// number prototype chain
//                Number.prototype    Object.prototype
//                      |                   |
// someNumber --------> * ----------------> * -------------> null
```

```javascript
// someArray    -> Array.prototype ---+
//                                     \
// someFunction -> Function.prototype --+-> Object.prototype ---> null
//                                     /
// someString   -> Sring.prototype ---+
//                                   /
// someObject ----------------------+
```

note:
	`someObject` has `Object.prototype`  directly above it in its prototype chain

some methods or properties may exist on both `SomeConstructor.prototype` and `Object.prototype`. 
These methods or properties are accessed first as they exist earlier in the prototype chain
This allows for custom implementations of certain methods


```javascript
// Object.prototype.toString()
console.log({}.toString());

// custom toString implementations 
console.log((function(a, b){ return a + b }).toString());
console.log([1, 2, 3].toString());
console.log(new Set([0, 0, 1, 2, 3, 3]).toString());
console.log(new Map([[0, 0], [1, 2], [3, 3]]).toString());
```

***INTERESTING***
Constructors are themselves functions.
Any function can be used as a function constructor (bar arrow functions).
hence function constructors are just regular functions.
this in turn means they are objects.

as objects they have their own properties and methods
properties and methods defined on the function constructors themselves are called static methods. e.g. (`Array.isArray`, `Object.keys`)

properties and methods defined on constructor instances (or in the prototype chain of the instances) are called instance methods

instances methods are found on the prototype property of the constructors 

functions constructors are functions and hence subject to the function prototype chain. 
`someFunction` could be  `Array`, `Map`, `Set`.

```javascript
// function prototype chain
//             Function.prototype    Object.prototype
//                     |                   |
// someFunction -----> * ----------------> * -------------> null


function validChain(someConstructor){
  return (
    someConstructor.__proto__ === Function.prototype
    || Function.prototype.__proto__ === Object.prototype
    || Object.prototype.__proto__ === null
  );
}

console.log(validChain(Array));
console.log(validChain(Set));
console.log(validChain(Map));
console.log(validChain(Symbol));
console.log(validChain(String));
```

This mean function constructors themselves are instances instances of the `Function` function constructor. 
Ergo like regular function they inherit `call`, `bind`, `apply`, `length` etc. as instance methods from `Function.prototype`
they also inherit `hasOwnProperty`, `isPrototypeOf` from `Object.prototype` as instance methods too

This leads to a distinction. 
Static methods and properties are those defined as own properties of a constructor. 
`Array.call`, `Object.length` aren't considered static methods

`Function` and `Object`

again constructor functions are functions and objects
this means that `Function.prototype` and `Object.prototype` are in their prototype chain
this fact is interesting when considering the `Function` and `Object` constructor functions
it means their own prototype properties are in their own prototype chains
making them instances of themselves

`someFunction` could also be the `Function` constructor function.
hence the `[[Prototype]]` of `Function` is its own prototype property
This means... ***hits blunt*** ...`Function` is an instance of itself??
`Function = new Function()` ???

note:
	`objA instanceof objB` return true if `objB.prototype` is on the prototype chain of `objA`

```javascript
// function prototype chain
//             Function.prototype    Object.prototype
//                     |                   |
// Function ---------> * ----------------> * -------------> null

console.log(Function.__proto__ === Function.prototype); // true
console.log(Function.__proto__.constructor === Function); // true
console.log(Function.prototype.isPrototypeOf(Function)); // true
console.log(Function instanceof Function); // true
```

`Object` is also just as curious
just like with `Function` its own prototype property it in its prototype chain.
therefore `Object` is an instance of itself

```javascript
// function prototype chain
//             Function.prototype    Object.prototype
//                     |                   |
// Object -----------> * ----------------> * -------------> null

console.log(Object.__proto__ === Function.prototype); // true
console.log(Object.__proto__.__proto__.constructor === Object); // true
console.log(Object.prototype.isPrototypeOf(Object)); // true
console.log(Object instanceof Object); // true
```

These also mean that `Object` is an instance of `Function` and `Function` is an instance of `Object`

```javascript
console.log(Function instanceof Object);
console.log(Object instanceof Function);
```


note:
	it is important to remember that `Object` and `Function` themselves aren't in their own prototype chains directly. Their `prototype` properties are.
	To say an `objectA` is an instance of `objectB` in JavaScript is to say that `objectB.prototype` is somewhere in the prototype chain of `objectA`

***BRUHHHH***


changing native prototype
though not recommended properties and methods can be added to the prototype property of native function constructors

prototypes are global hence conflicts are easily possible

```javascript
String.prototype.show = function() {
  console.log(this);
};

"BOOM!".show(); // BOOM!
```


the feature is used for polyfilling. 
Polyfilling is a term for making a substitute for a method that exists in the JavaScript specification, but is not yet supported by a particular JavaScript engine.

borrowing from prototypes
methods from native constructor prototypes can be applying to objects that are not instances of that constructor.
this can be done using `call`, `bind` or `apply` and passing the target object as the `this` argument
or by copying the desired method onto the target object and letting the object before the dot rule automatically assign the target object as this

```javascript
const obj = {
	0: "h",
	1: "e",
	2: "l",
	3: "l",
	4: "o",
	length: 5
};

console.log(Array.prototype.join.call(obj, "+"));

obj.join = Array.prototype.join;
console.log(obj.join("->"));
```

read later
function caller accessor property
[MDN instanceof](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/instanceof)