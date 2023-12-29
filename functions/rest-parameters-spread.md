---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/rest-parameters-spread
banner: "![[424098.jpg]]"
---
The rest parameter and spread syntax allows functions to support an arbitrary number of arguments  similar to `Math.max`, `Object.assign`

rest syntax
note:
	JavaScript allows more arguments to be passed into a function than the function has parameters
	the extra arguments are ignored.

Rest parameters allow the argument surplus to be gathered into an array

```javascript
function prodThenSum(a, b, ...rest){
	const prod = a * b;
	const sum = rest.reduce((p, v) => p + v);
	return { prod, sum };
}

prodThenSum(3, 2, 3, 4, 5, 5); // prod: 6, sum: 17
```

warning that the rest parameters must come at the end of the parameter list.

info: `arguments`
	the `arguments` keyword is an [[iterable|array-like]] iterable that contains all the arguments passing into a function.

info: 
	[[arrow-functions-basics|arrow functions]] have no `arguments`
	they take their `arguments` from the outer function
	similar to `this`

warning:
	calling arguments outside a function in the browser results in an error
	in Node it prints out an object (interesting)

the spread syntax allows arrays to be deconstructed and each values passed in as an individual argument to a function

the arguments are passed in the order in which they appear in the array

```javascript
const random = (min, max) => ~~(Math.random() * (max - min) + min);
const arr0 = Array(10).fill(0).map(() => random(0, 1E3));
const arr1 = Array(10).fill(0).map(() => random(0, 1E3));
Math.max(random(0, 1E4), ...arr0, random(0, 1E5), ...arr1);
```

tip:
	any iterable object works with the spread syntax
	i.e. `string`, range object, `Object.entries` return value 
	`[..."hello world"]`

note:
	`Array.from` works with both array-likes and iterables ([[iterable|see difference]]).
	`...spread` only works on iterables

Copying array/object
The spread syntax allows for the creation of [[object-copy#Shallow Copies|shallow copies]] of objects and arrays

`const newArr = [...arr0, middleValue, ...arr1]`
`const newObject = { ...obj0, ...obj1 }`

```javascript
const random = (min, max) => ~~(Math.random() * (max - min) + min);
const arr0 = Array(10).fill(0).map(() => random(0, 1E3));
const arr1 = Array(10).fill(0).map(() => random(0, 1E3));
const arr2 = [...arr1, ...arr2]; // spread for merging
```