---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/static-properties-methods
---
static properties are properties  defined on a class itself 
they are properties of the functions themselves 
the `static` keyword is used to defined static properties in the class syntax

```javascript
class SomeClass {
	static staticProperty = 42;
	static staticMethod(){
		console.log(this === SomeClass);
	}
}

SomeClass.staticMethod();
console.log(SomeClass.staticProperty); // 42
```

the `static` keyword is again syntactic sugar for assigning the property to the corresponding function

```javascript
function SomeClass(){}
SomeClass.staticProperty = 42;
SomeClass.staticMethod = function(){
	console.log(this === SomeClass);
}

SomeClass.staticMethod();
console.log(SomeClass.staticProperty); // 42
```

static properties are accessed through the class on which they are defined not the instances. (they are properties of the class itself after all).

static properties are useful for storing data that is general to all instances or independent to any specific instance.
number of instances created
keep track of instance id

static methods are useful for implementing logic which is general to all instances of the class.
a method to compare two instances 
a method to create custom instances

Inheritance

Static properties are inherited between classes

```javascript
class Parent {
	static foo = 42;
}

class Child extends Parent {}

console.log(Child.foo); // 42
```

when `Child` extends `Parent` its `[[Prototype]]` becomes a reference to `Parent`
this is in addition to `Child.prototype.[[Prototype]]` being set to `Animal.prototype`


```javascript
// Child
//   |
//   V
// Parent
//   |
//   V
//   + ----- Function.prototype
//   |
//   V
//   + ----- Object..prototype
//   |
//   V
//  null


class Parent {}
class Child extends Parent {}

console.log(Child.__proto__ === Parent); // true
console.log(Child.__proto__.__proto__ === Function.prototype); // true
console.log(Child.__proto__.__proto__.__proto__ === Object.prototype); // true
console.log(Child.__proto__.__proto__.__proto__.__proto__ === null); // true
```

this creates an interesting prototype chain
recall that in JavaScript `objectA` is an instance of `objectB` means that `objectB.prototype` is somewhere in the prototype chain of `objectA`.
ergo `objA instanceof objB` return true if `objB.prototype` is on the prototype chain of `objA`.
this means that `Child` is not an instance of `Parent`.
instances of `Child` are instances of `Parent` but `Child` itself is not

```javascript
class Parent {}
class Child extends Parent {}

console.log(Child instanceof Parent); // false
console.log(new Child() instanceof Parent); // true
```