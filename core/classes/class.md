---
tags:
  - backend
  - draft
  - javascript
  - jsinfo
  - frontend
source: https://javascript.info/class
---
A class is used as a blueprint for object creation

The basic syntax is shown below

```javascript
class ClassName {
	constructor(args){}
	method0(){}
	method1(){}
	methodN(){}
}

const newObject = new ClassName(...args);
```

note:
	no separating commas between methods in the class body

note:
	the `constructor` can be omitted. 
	if it is a default constructor roughly equivalent to `constructor(){}` is used

when a class is instantiated using the `new` key word
- a new empty object is created.
- the `constructor` method is called automatically passed the given arguments.
- the new object is assigned to `this` within the constructor 
the makes the constructor the perfect place to initialize and object

```javascript
class Person {
  constructor(name, age, isMarried, height, gender){
    this.name = name;
    this.age = age;
    this.isMarried = isMarried;
    this.height = height;
    this.gender = gender;
  }

  introduce(){
    return (
      `Good day! My name is ${this.name}. ` 
      + `I am a ${this.age} year old ${this.gender}`
    )
  }
}


const seun = new Person("Jesuseun", 22, false, 191, "male");

console.log(seun);
console.log(seun.introduce());
```

A class is just syntactic sugar for a function constructor.
the class definition compiles down to a function named after the class.
the body of this function is take from the `constructor`
next the methods defined within the class body are copies into the prototype property of the created function


```javascript
console.log(typeof Person); // "function"
console.log(Person); // "[class Person]"
console.log(Object.prototype.toString.call(Person)); // [object Function]
console.log(Object.getOwnPropertyDescriptors(Person.prototype));

function validChain(someConstructor){
  return (
    someConstructor.__proto__ === Function.prototype
    || Function.prototype.__proto__ === Object.prototype
    || Object.prototype.__proto__ === null
  );
}

// Person class has the same prototype chain as function constructors
validChain(Person); // true
```

recall that `prototype` property of function constructors (ergo all functions) by default contains a `constructor` property that is a reference to the function constructor itself.
the `Person.prototype` also contains a `constructor` function referring `Person` itself.


class methods are not just syntactic sugar for function constructors
functions created using `class` have a `[[IsClassConstructor]]: true` property
functions with this property set to `true` must be called with `new`
if `class` keyword is used in a file the file is automatically set to strict mode 
Class methods all have `enumerable: false

```javascript
class PersonClass {
  foo(){}
}

function PersonFunc(){}
PersonFunc.prototype.foo = function(){}

console.log(Object.getOwnPropertyDescriptors(PersonClass.prototype));

console.log(Object.getOwnPropertyDescriptors(PersonFunc.prototype));
```
`
note:
	When assigning methods and properties to `Func.prototype` don't reassigned the whole object unless using `Object.create` or `Object.assign` to copy all existing data


class expression
classes are functions and hence are values
they can be passed around, returned, assigned etc.

```javascript

const Person = class {
  constructor(name){
    this.name = name
  }
}

const person = new Person("Jesuseun");

console.log(Person);
```

named class expressions are also possible
dummy name is only visible from within the class
similar to functions classes also have the contextually aware `name`  property
`name`is read-only

```javascript
const Person = class DummyName{
  constructor(name){
    this.name = name
  }
}

console.log(Person.name); // DummyName
```

classes can be dynamically generated using functions
sounds similar to python meta-classes 

classes also support getters an setters and computed properties
the syntax is identical to that used in object literals

```javascript
class Person {
  constructor(firstName, lastName){
    this.firstName = firstName;
    this.lastName = lastName;
  }

  get fullName(){
    return `${this.firstName} ${this.lastName}`;
  }

  set fullName(value){
    [this.firstName, this.lastName] = value.split(" ");
  }

  [Array(10).fill(0).map(() => + String(~~(Math.random() * 10))).join("")] (){
    console.log("randomly named method");
  }
}

const person = new Person("Jesuseun", "Olatunde");

console.log(person); // firstName lastName

console.log(Object.getOwnPropertyDescriptors(Object.getPrototypeOf(person)));
// fullName accessor property
// randomlyNamed property
```

note:
	accessor properties are stored on the prototype property of the class just like regular methods


class fields
previously only methods could be specified in the class body.
these methods are copied and stored in the `[[Prototype]]` of the instantiated object
not on the object themselves

class fields allow properties to be specified as well.
class field are stored on the objects themselves.
as with the object literal syntax all attributes default to `true`

```javascript
class Person {
	id = 0;
}

const person = new Person();
console.log(Object.getOwnPropertyDescriptors(person));
```

note
class field can be any expression.
the expression is reevaluated each time the class is instantiated 
this means that objects created in the class field expression can never be `===` between instances

note
methods between instances are always equal
they are places on  `SomeClass.prototype`
the `[[Prototype]]`  property of instantiated objects are just memory references to `SomeClass.prototype` 


```javascript
class Person {
	id = ~~ (Math.random() * 10);
	testObj = { name: "lorem" };
  foobar(){}
}

const person0 = new Person();
const person1 = new Person();

console.log(person0); // 5
console.log(person1); // 7

console.log(person0.testObj === person1.testObj); // false
console.log(person0.foobar === person1.foobar); // true
```


class field can be used to create bound methods when combine with the function expressions (specifically arrow functions).
these bound methods always have their `this` value referencing the instance on which they are defined regardless of the context in which they are called

```javascript
class Button {
	value = ~~ (Math.random() * 10);
	click = () => console.log(this.value);
}

setTimeout(btn0.click, 1000); // 9
setTimeout(btn1.click, 2000); // 6
setTimeout(btn2.click, 3000); // 7
```

note
this does not work with regular function expressions
the arrow function takes `this` from the outer scope
implies that unlike object literal `this` within the `{}` of a class definition is the instance

```javascript
class Button {
	value = ~~ (Math.random() * 10);
	click = function(){ console.log(this.value) }
}


setTimeout(new Button(), 1000); // undefined
setTimeout(new Button(), 2000); // undefined
setTimeout(new Button(), 3000); // undefined
```


note:
methods defined using class field and function expressions are stored on the instances not in the `[[Prototype]]` of the instance.

```javascript
class Button {
	value = ~~ (Math.random() * 10);
	click = function(){ console.log(this.value) }
}

console.log(new Button().click === new Button().click);
```

further reading
python meta-classes
possible patterns for generating dynamic classes with JavaScript
