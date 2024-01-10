---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/destructuring-assignment
---
*Destructuring assignment* syntax allows for the unpacking of arrays or objects.

Array destructuring 

```javascript
[
	variablePropertyParameterName = defaultExpression, 
	/*omittedElement*/, 
	...restArray
] = array;
```

```javascript
// original array is left unaltered
// ignores the remaining elements
const [A, B, C] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ".split("");
console.log(A, B, C); // "A" "B" "C"

// works with any iterable i.e strings
const [A, B, C] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

// ignore elements using commas,
const [A, , C] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

// assign to anything 
const abcObj = {}
[abcObj.A, abcObj.B, abcObj.C] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

// use it in destructing values in for..of loops i.e Object.entries()
for(const [key, value] of Object.entries(abcObj)){
	// 1. key === "A", value === "A"
	// 1. key === "B", value === "B"
	// 1. key === "C", value === "C"
}

// use to swap vairables 


```

rest syntax  `...` can be used to gather the unused elements into an array

```javascript
const [A, B, C, ...others] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ".split("");
console.log(A, B, C, others); // "A" "B" "C", ["D", "E", "F", ...];
```

default values can be used an are similar to [[function-basics#Default Parameters]] 
the default can be any expression and is only evaluated in the absence of a value

```javascript
let [randAge = ~~(Math.random() * 100)] = [];
let [givenAge = ~~(Math.random() * 100)] = [22];
```

Object destructing

```javascript
const {
	propertyKey: variableParameterPropertyName = defaultExpression,
	...restObject
} = object
```

```javascript
const person = { firstName: "J", lastName: "O", age: 22}
const {firstName, lastName: lName, age} = person

// order does not matter
// specify variable name if different from property name
// default values are support similar to array destructuring 

const {width: w = 100, height: h = 100, title} = { title: "TV Size"};
w; //  100
h; // 100
title; // "TV Size"

// use ...rest syntax to gather unused properties into a new object
```

warning:
	if `let` or `const` are not used during object destructuring JavaScript treats the curly braces as a code block and throws and error.
	surround the assignment in `())` to indicate an expression

nested destructuring 
if the value has nested properties and a complex structure the same pattern can be used on the assignment side to destructure it

```javascript
let options = {
  size: {
    width: 100,
    height: 200
  },
  items: ["Cake", "Donut"],
  extra: true
};

// destructuring assignment split in multiple lines for clarity
let {
  size: { // put size here
    width,
    height
  },
  items: [item1, item2], // assign items here
  title = "Menu" // not present in the object (default value is used)
} = options;

alert(title);  // Menu
alert(width);  // 100
alert(height); // 200
alert(item1);  // Cake
alert(item2);  // Donut
```

smart function parameters
if a function has multiple parameters it can be difficult to remember the parameter order and pass in arguments in the right order
the parameter can instead be made an object whose properties are the intended parameters 
the object destructuring syntax can then be used to assign this properties to parameter variables
renaming and default values can also be used 
now an object is passed as an argument and the parameter order is irrelevant

array destructuring can also be used in function parameters 

```javascript
function myFunc({
	incomingProperty: varName = defaultExpression,
	...
} = defaultObject){
	// code
};
```

note:
	the function always expects an object as an argument.
	to use all default arguments pass in an empty object `myFunc({})`
	or assign a default object using function default parameter syntax

note:
accessor properties can be destructured 

```javascript
const testObj = {
  firstName: "Jesuseun",
  lastName: "Olatunde",

  get fullName(){
    return this.firstName + " " + this.lastName;
  }
}

function printFullName({ fullName }){
  console.log(fullName);
}

printFullName(testObj); // "Jesuseun Olatunde"
```