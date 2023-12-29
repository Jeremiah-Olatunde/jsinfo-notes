---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/object-methods
---
# Object Methods

Objects model real world items. These items have characteristics like age, mass, names. These characteristics are modelled with properties. These items can also have actions associated with them. A person can speak, a rabbit can run. These are modelled with functions as property values also known as methods.

---
# Syntax

Methods can be created with the function expression syntax.

```javascript
const person = { name: "Jesuseun Jeremiah Olatunde" };

person.sayHi = function(){ 
	console.log("Hi!"); 
};
```

Functions previously declared with the function declaration syntax can also be assigned to a property making it a method.

```javascript
function sayBye(){
	console.log("Bye");
}

person["sayBye"] = sayBye;
```

Methods can be declared within object literals just like any other value. However the value in this case is a function expression.

```javascript
function sayBye(){
	console.log("Bye");
}

const person = {
	name: "Jesuseun Jeremiah Olatunde",
	sayHi: function(){ console.log("Hi"); },
	sayBe, // Using [[object#Shorthand]]
}
```

There exists also a method shorthand syntax.

```javascript
const person {
	name: "Jesuseun Jeremiah Olatunde",
	sayHi(){
		console.log("Hi");
	},
}
```

---
# `this`

Methods have access to the object on which they are defined via the `this` keyword. The value of `this` is always the object on which the method is called (i.e. the object before the dot). 

```javascript
const person = {
	name: "Jesuseun",
	greetFormally: function(){
		console.log(`Hello, my name is ${this.name}`);
	} 
};

person.greetFormally(); // "Hello, my name is Jesuseun"
```

> [!info] A sketchy alternative
> The object on which the method is defined can also be accessed via the outer variable but this is unreliable and has its caveats. This however creates problems when the variable is reassigned.
> ```javascript
> let person = {
> 	name: "Jesuseun",
> 	["sayHi"]() {
> 		console.log(person.name); // accessing name through person variable
> 	}
> }
> const user = person;
> person = null;
> user.sayHi(); // sayHi method is still trying to read person.name (i.e undefined.name)
> ```
> >[!tip] `const` to save the day?
> >using `const` in the declaration of `person` prevents reassignment.

---
# `this` is free

The `this` keyword can be used inside any function regardless of if it is a method or not. The value of `this` is computed at run time and depends on the context. If a function is assigned to two different objects the value of `this` changes based on which object the function (now method) is called on.

```javascript
function sayHi(){
	console.log(this.name); // this used in a regular function
}


// sayHi function assigned to both seun and seni object with different values for the name property

const seun = { name: "Jesuseun Jeremiah Olatunde", sayHi };
const seni = { name: "Jesuseni Nehemiah Olatunde", sayHi };

// the value of this becomes the seun object.
// therefore this.name === "Jesuseun Jeremiah Olatude"
seun.sayHi(); // "Jesuseun Jeremiah Olatunde"

// the value of this becomes the seni object
// therefore this.name === "Jesuseni Nehemiah Olatunde"
seni.sayHi(); // "Jesuseni Nehemiah Olatunde" 

```

> [!note] Functions are copied by reference
> Functions are objects and are therefore [[object-copy#Copying by reference|copied by refence]]. Hence in the above example the exact same function object is being called as a method by two different objects: yet the value of `this` used in the function changes to become the object before the dot at the time of invokation.
> ```javascript
> console.log(seun.sayHi === seni.sayHi); // true
> ```

> [!warning] `undefined` `this`
> If a function referencing `this` is not invoked as a method the value of `this` becomes `undefined` in [[strict-mode|strict mode]] or the [[global-object]] in non-strict mode.
> ```javascript
> sayHi(); // undefined in strict
> sayHi(); // globalThis non-strict
> ```

> [!note] Toji `this`
>  In other programming languages `this` is bound. The value of `this` in a method is always the object on which the method was defined. Even if the method is moved to another object. However `this` in JavaScript, like Toji, is free. 
>  > [!quote] 
>  > They would all bear witness to the bare flesh of the one who is free.

---
# Arrow functions

[[arrow-functions-basics|Arrow Functions]] do not have their own `this` value and instead take `this` from its immediate outer environment. If an arrow function is defined in the global scope `this` becomes [[global-object]]

```javascript
const person = {
	name: "JJO",
	arrow: () => this,
	wrappedArrow: function(){
		const sayHi = () => console.log("Hello from", this.name);
		sayHi();
	},
	weird: () => console.log(this),
}

// the value of this in wrappedArrow becomes person
// sayHi arrow function is defined in wrappedArrow (its immediate outer environment)
// this in sayHi becomes person as well
person.wrappedArrow(); // "Hello from JJO"

// top level this is the global object (window in browser environments);
console.log(this === (() => this)()); // true
console.log(this === person.arrow()); // true
```

> [!note] 
> When `this` is the global object non-existent properties become `undefined` hence there is no error in strict mode
> ```javascript
> const user = {
>   name: "lorem ipsum",
>   greet: () => { console.log("hello from", this.name); }
> }
> // this is the global object
> user.greet(); // hello from undefined
> const greet = user.greet;
> greet(); // hello from undefined
> ```
> If a function expression were used instead `this` would be `undefined` and an error would be thrown
> ```javascript
> const user = {
>   name: "lorem ipsum",
>   greet: function(){ console.log("hello from", this.name); }
> }
> 
> user.greet(); // hello from lorem ipsum
> const greet = user.greet; 
> greet(); // can not access property of undefined
> ```

> [!question] Node environment weirdness
> Why does the code below print out an object object.
> ```javascript
> person.weird(); // {} outputs empty object
> console.log(this === (() => this)()); // yet somehow this is still true
> console.log(this === person.arrow()); // and this as well true
> ```

---
# Tasks

#coding-problems/javascript/jsinfo 

What is the result of accessing `ref`?  Why?

```javascript
function makeUser() {
	/* 
		the value of this inside makeUser when invoked as a function and not a method
		is undefined in strict mode, and global object in non strict mode.
	*/ 
	// this === undefined || this === globalThis
	return {
		name: "John",
		ref: this // ref value becomes undefined in strict mode
	};
}

// invoking makeUser not as a method 
let user = makeUser(); 

// in strict more ref is undefined
// result is cannot access property of undefined
alert( user.ref.name ); 

// in non-strict mode this is global object
// the global object may or may not have a name property
// so the result is the name value or undefined
alert( user.ref.name ); // name or undefined
```