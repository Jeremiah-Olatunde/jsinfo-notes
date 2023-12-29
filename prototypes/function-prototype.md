---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/function-prototype
---
new object can be created using the function constructor syntax [[constructor-new]]
`new F()`
the function `F` has a property called `prototype`
when making an object using `new F()` the `prototype` property of `F` becomes the `[[Prototype]]` of the newly created object

```javascript
const person = { name: "Jeremiah" };
const other = { name: "decoy" };

function Athelete(){}
Athelete.prototype = person;

const athelete = new Athelete;
console.log(athelete.name);
```

note:
	the distinction between the prototype property of the function constructor `F.prototype` and the prototype of the function constructor `[[Prototype]]`

note:
	`F.prototype` is only used during instantiation
	if the object it references is changed after an object has been created the `[[Prototype]]` of the will continue to reference the original `F.prototype` object
	new objects however will reference the new object

```javascript
const person = { name: "Jeremiah" };
const other = { name: "decoy" };

function Athelete(){}
Athelete.prototype = person;


const athelete = new Athelete;

Athelete.prototype = other

console.log(athelete.name); // Jeremiah i.e. still person object

console.log(new Athelete().name) // decoy i.e other object
```

All functions have a default prototype
this is an object with a `constructor` property
this property is a reference to the function itself

therefore all instantiated objects have access to the constructor function from which they were created

```javascript
function Wabbit(){}
console.log(Wabbit.prototype.constructor === Wabbit);

const rabit0 = new Wabbit;
const rabit1 = new (rabit0.constructor);

console.log(rabit1.constructor === Wabbit)
```


note:
	the `constructor` property can be changed

tip:
	use `Object.freeze` to prevent his

```javascript
function Rat(){}
function Rabbit(){}

Object.freeze(Rabbit.prototype);

Rabbit.prototype.constructor = Rat; // Error 
```