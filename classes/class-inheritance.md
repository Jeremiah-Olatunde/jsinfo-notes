---
tags: 
source: https://javascript.info/class-inheritance
---
inheritance allows for the extension of classes 
creating new functionality on top of an existing class

```javascript
class ParentClass {}
class ChildClass extends ParentClass {}


```

note:
	`ParentClass` specifies either the class or an expression that evaluated a class
	technically specifying just the class is an expression that evaluates to a class

Again this is syntactic sugar for prototypal mechanics
the `[[Prototype]]` of `ChildClass.prototype` is set to `ParentClass.prototype` 
instances of `ChildClass` have their `[[Prototype]]` set to `ChildClass.prototype`
hence the prototype chain of the instance becomes:

```javascript
// new ChildClass()
//    |
//    V
//    + ----- ChildClass.prototype
//    |
//    V
//    + ----- ParentClass.prototype
//    |
//    V
//    + ----- Object..prototype
//    |
//    V
//   null
```

as such when property look up occurs if a property is absent from `ChildClass.prototype` it is obtained from  `ParentClass.prototype`

overriding a method
methods defined in the class body are stored in `ClassName.prototype`.
if a method is absent from `ChildClass.prototype` it is *"taken"* from `ParentClass.prototype`.
however if an identically named method is specified in `ChildClass.prototype` then that is used instead essentially override the method on `ParentClass`
basic prototype chain mechanics, the first occurrence in the chain is used.

`super(...args)` is used to call the parent constructor from within the child constructor
the `super.method(...args)` keyword is used to call the parent method from within a child method.

override constructor 
if the `ChildClass` constructor is omitted the default construct below is used

```javascript
class ChildClass extends ParentClass {
	constructor(...args){
		super(...args);
	}
}
```

if a constructor is specified in `ChildClass` it must call the parent constructor using `super(...)` before using `this` or before returning.
constructors on derived classes have `[[ConstructorKind]]: "derived"` property
derived constructors differ
when a class is instantiated with `new` a new object is created and assigned to `this`
when a derived class is instantiated this step is skipped and is expected to be carried out but the base class

overriding class fields 
class field are initialized:
- before the constructor in a parent class
- and after the call to super in a derived class

this means that the parent constructor always uses its own class field as they are yet to be overridden 

```javascript
class Animal { 
  foo = {
    createdAt: Date.now(),
    name: "animal",
  }; 

  constructor(){
	  // Animal class field are initialize before running constructor
	  // foo.name === animal
	  // class fields of Rabbit have not been intialized 
	  // hence foo has not yet been overrided
    console.log(this.foo);
  }
}

class Rabbit extends Animal { 
  foo = {
    createdAt: Date.now(),
    name: "rabbit",
  }; 

  constructor(){
    super();
    // now Rabbit class fields are initialize
    // foo overrides the foo on the this object created in Animal 
    // foo.name === rabit
    console.log(this.foo);
  }
}

const rabbit = new Rabbit();
```

note the first logged to console `createdAt` is earlier in time .

not that the class fields in the derived class have access to those of the parent class.
which makes sense as once the parent class fields are initialized they are just regular properties on `this` object

```javascript
class Animal { 
  createdAt = Date.now();

  constructor(){
	  // Animal createdAt class field initialzed and timestamped
	  // 2 second time delay
    const start = Date.now();
    while(Date.now() - start < 2000);
  }
}

class Rabbit extends Animal { 
  createdAt = Date.now() - this.createdAt;

  constructor(){
    super();
    // this = { createdAt: timestamp }
    // now Rabbit createdAt class field is initialized
    // expression uses the createdAt value present on this
    // then over writes it
    console.log(this.createdAt); // 2000
  }
}

const rabbit = new Rabbit();
```

the behavior is different for methods
with methods it is the derived class method that is called even in the parent constructor
as soon as the new empty object is created (which happens before the parent constructor is run) its `[[Prototype]]` is set to the `prototype` of the derived class.
hence calling `this.method()` searches the prototype chain and finds the method defined on the derived class first


super internals

note:
trying to implement super by manually calling a method on the `[[Prototype]]` and binding `this` (i.e. `this.__proto__.method.call(this)`).

```javascript
let animal = {
  name: "Animal",
  eat() {
    console.log(`${this.name} eats.`);
  }
};

let rabbit = {
  __proto__: animal,
  eat() {
    // this === longEar (as it was bound manually with call)
    // ergo this.__proto__ === rabbit 
    // calling rabbit.eat but binding this to longEar
    // infinite recursion
    this.__proto__.eat.call(this); // (*)
  }
};

let longEar = {
  __proto__: rabbit,
  eat() {
    // this === longEar and __proto__ === rabbit
    // calling rabbit.eat but binding this to longEar
    this.__proto__.eat.call(this); // (**)
  }
};

longEar.eat();
```

if the method was called without binding `this` recursion would be avoided but  `this` in the method calls would point to the object on which the currently running method is defined.
any modifications made to it after only that object and not the original

```javascript
let animal = {
  name: "animal",
  eat() { console.log(`${this.name} eats.`); }
}


let rabbit = {
  name: "rabbit",
  __proto__: animal,
  eat() { this.__proto__.eat(); }
};



let longEar = {
  __proto__: rabbit,
  name: "long",
  eat() { this.__proto__.eat() } 
};

longEar.eat(); // animal eats
```

internal solution
all methods defined in a class or an object have an internal property called `[[HomeObject]]`
this property stores the object on which the method was originally defined
if a method is moved around its `[[HomeObject]]` remains unchanged
`super` makes use of this home object property in order to ascend the prototype chain
when super is used in a method the engine takes its `[[HomeObject]]` and gets the `[[Prototype]]` of the `[[HomeObject]]

```javascript
let animal = {
  name: "animal",
  eat() { console.log(`${this.name} eats.`); }// [[HomeObject]] === animal
};

animal.eat.home = animal;

let rabbit = {
  name: "rabbit",
  __proto__: animal,
  eat() { super.eat(); } // [[HomeObject]] === rabbit
};

rabbit.eat.home = rabbit;


let longEar = {
  __proto__: rabbit,
  name: "long",
  eat() { super.eat() } // [[HomeObject]] === longEar
};
```

> [!warning]
> Function properties i.e. method defined using function literals lack a `[[HomeObject]]` property. Ergo `super` can not be used within these methods.
>
> ```javascript
> let animal = {
> 	name: "animal",
> 	eat() { console.log(`${this.name} eats.`); }
> }
>
>let rabbit = {
>	name: "rabbit",
>	__proto__: animal,
>  // function property
>  // Syntax Error
>  eat: function() { super.eat(); } 
>};
>
>let longEar = {
>	__proto__: rabbit,
>	name: "longEar",
>	eat() { super.eat() } 
>};
>```


this implies that methods are not truly free. when they are moved around their `[[HomeObject]]` remains the same, it is forever bound to the object on which it was defined.

```javascript
let animal = {
  sayHi() {
    alert(`I'm an animal`);
  }
};

// rabbit inherits from animal
let rabbit = {
  __proto__: animal,
  sayHi() {
    super.sayHi(); // super always take prototype of [[HomeObject]]
  }
};

let plant = {
  sayHi() {
    alert("I'm a plant");
  }
};

// tree inherits from plant
let tree = {
  __proto__: plant,
  sayHi: rabbit.sayHi // [[HomeObject]] of rabbit.sayHi remains rabbit
};

tree.sayHi();  // I'm an animal 
```