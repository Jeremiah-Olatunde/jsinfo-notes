---
tags:
  - backend
  - javascript
  - jsinfo
  - frontend
source: https://javascript.info/constructor-new
---
The `new` keyword allows for the creation of multiple objects.

---
# Constructor function

Constructor functions allow for implementation of reusable object creation code. They enable objects with similar blue prints to be created quickly and easily without repeating boiler plate code.  By convention their names begin with a capital letter and are intended only to be executed with the `new` keyword.

When a function is called with the `new` keyword the follow steps occur:
1. an empty object `{}` is created and assigned to `this` implicitly.
2. The function body executes: Usually using the arguments provided and custom logic to modify the `this` value and create a new object
3. `this` (now modified) is returned implicitly.

```javascript
// function constructor for user objects
function CreateUser(name, age, isAdmin){
	// this = {} implicity
	this.name = name;
	this.age = age;
	this.isAdmin = isAdmin;
	
	// Arrow functions take the this value of their enivornment 
	this.promote = () => this.isAdmin = true;
	
	// return this
}

// creating instances of user
const user0 = new CreateUser("Jesuseun", 22, true);
const user1 = new CreateUser("Jesuseni", 18, false);
```

> [!warning] Bad Style 
> When using the new keyword, function constructors without arguments can be called without the parentheses 
> ```javascript
> function CreateUser(){
> 	this.isAdmin = false;
> 	this.promote = () => this.isAdmin = true;
> }
> 
> const user0 = new CreateUser;
> ```

> [!warning] No arrow constructors
> Arrow functions lack their own `this` value and as such can not be used as constructor functions.

> [!tip] 
> `new` can be used with the function expressions syntax to neatly wrap the creation of a single object and avoid polluting the outer scope. note how the `new` keyword automatically invokest the function
> ```javascript
> const seun = new function(){
> 	// complex creation logic variables or functions only used once
> 	const randomNumber = (min, max) => ~~(Math.random() * (max - min) + min);
> 	this.name = "Jesuseun Jeremiah Olatunde";
> 	this.age = 22;
> 	this.idNumber = randomNumber(1000, 10000);
> }
> ```
> 

---
# `new.target`

Inside a function the `target` property of `new` can be used to determine whether or not the `new` keyword was used during invocation of a function. If a function is called with `new`, `new.target` is equal to that function, otherwise it is `undefined`.

> [!example]
> `new.target` can be used to throw and error if a function was called incorrectly
> ```javascript
> function CreateObj(){
> 	if(!new.target) throw new Error("Must call this function with new");
> }
> const obj = new CreateObj; // no error
> const objErr = CreateObj; // Error("Must call this function with new")
> ```
> `new.target` could also be used to automatically call the function constructor with `new` if it was not originally
> ```javascript
> function CreateObj(){
> 	if(!new.target) return new CreateObj;
> }
> const obj = new CreateObj; // new obj
> const objErr = CreateObj; //  also new obj
> ```

---
# Return from constructors

If an explicit `return` statement is specified within a constructor function one of two things happen:
- If the returned value is a [[types|primitive]] then it is ignored and `this` is returned as normal.
- If the return value is an object then that is return instead of `this`. (pun intended).

```javascript
function MaybeDupeConstructor(name, age){
	this.name = name;
	this.age = age;
	const dupe = { 
		name: "Sike, nigga you thought", 
		age: 69 
	};
	const primitive = Math.floor(Math.random() * 100);
	// if return value is primitive then this is returned
	// if it is dupe then dupe is returned
	return Math.random() < 0.5 ? primitive : dupe;
}

const person0 = MaybeDupeConstructor("Jeremiah", 22); // 81
const person1 = MaybeDupeConstructor("Jeremiah", 22); // { name: "Jeremiah", ...}
```

