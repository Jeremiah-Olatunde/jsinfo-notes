---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/object
banner: "![[RDT_20211231_0543322971168337628060114.jpg]]"
---
# Objects

Objects are a collection of properties. A property is a `key: value` pair where each key is a `string` or [[symbol|`symbol`]] and the value can be of any type. 

---
## Object creation

Objects can be created using the object literal syntax which uses the `{}` along with an optional list of properties within `{}` separated by a `,`. Object can also be created using the object constructor syntax.

```javascript
// Object literal syntax
const person0 = {
	"first name": "Jesuseun",
	"middle name": "Jeremiah",
	"last name": "Olatunde"
};

// Object constructor syntax
const person1 = new Object();
```

> [!note] Valid Identifier Keys
> If the `string` used for an object key is also a [[variables|valid variable identifier]] then the quotations may be omitted.
> ```javascript
> const person0 = {
>	firstName: "Jesuseun",
>	middleName: "Jeremiah",
>	lastName: "Olatunde"
>};
>```

>[!info] Special Keys
>
>`number` values may be used as property keys however they are coerced into `string`. These are called *integer properties*. They can only be accessed using `[]` syntax.
>```javascript
>const numObj = {
>  0: "h", // stored as "0": "h"
>  1: "e",
>  2: "l",
>  3: "l", 
>  4: "o"
>};
>console.log(numObj[0]); // 0 is coerced into "0"
>```
>Reversed keywords like `let`, `for`, `while` can also be used as keys. 
>```javascript
>const obj = { let: 0, for: 1, return: 2}
>console.log(obj.let, obj["for"], obj.return);
>```

> [!warning]
> `__proto__` key can not be assigned to a primitive value


---
## Accessing properties

> [!note]- Accessing non-existent properties
> If a property that does not exist is accessed JavaScript returns `undefined`.
### Dot notation

Property values can be accessed using the dot notation syntax ( `objectName.propKey`) which only works if the property key is a valid identifier. 

```javascript
const person = {
	"first name": "Jesuseun",
	"last name": "Olatunde",
	age: 22,
	height: 190,
	married: false,
};

// Dot notation syntax
console.log(person.age); // 22 
console.log(person.height); // 190
console.log(person.first name); // FAILS!!

person.height += 20; // accessing height property and modifying the value
delete person.married; // deleting the married property

console.log(person.married); // undefined;
console.log(person.height); // 200
```

### Square bracket syntax 

If the string used for the key is not a valid identifier then the `[]` syntax (`objectName[expression]`) must be used. The expression within the `[]` is evaluated and the resulting value is coerced into a `string`. 

```javascript
const person = {
	"first name": "Jesuseun",
	"last name": "Olatunde",
	age: 22,
	height: 190,
	married: false,
};

// Square bracket syntax
// "first name" is an invalid identified due to the space
console.log(person["first name"]); // "Jesuseun" 
person["first name"] = "Seun"; // Modifying "first name" property
delete person["first name"]; // deleting "last name" property

function randomProp(){
	const props = [
		"last name", "age", 
		"height", "married",
	];
	return props[Math.floor(Math.random() * props.length)];
}

// Any expression can be used within the square brackets
console.log(person["last" + " " + "name"]); // "Olatunde"

console.log(person[randomProp()]) // false
console.log(person[randomProp()]) // 190
console.log(person[randomProp()]) // "Olatunde"

delete person[randomProp]; // delete a property randomly
```

---
## Computed properties

`[]` can be used inside the object literal syntax when creating properties. Properties created this way are called ***computed properties***. This allows for the use of complex expressions when creating the property key.

```javascript
const person = {
	["first" + " " + "name"]: "Jesuseun",
	[prompt("what did you buy") || "nothing"]: prompt("how many did you buy") || 0
}
```

---
## Shorthand

When using the object literal syntax if the property key has the same name as a variable in scope that contains the desired value only the property key need be specified.

```javascript
function makePerson(firstName, lastName, year){
	return {
		firstName, // instead of firstName: firstName,
		lastName, // instead of lastName: lastName,
		age: new Date().getFullYear - year, // combine shorthand and normal syntax
	}
}
```

---
## `in`

The `in` operator is used to test for the existed of a property in an object like so `propKey in obj`.
It returns `true` if the property exists and `false` if not. 

> [!warning]
> The left operator must be a `string` or an expression that evaluates to a string
> ```javascript
> const key = "firstName";
> const person = { firstName: "Jesuseun" };
> console.log(key in person);
> console.log(firstName in person); // ERROR firstName not defined
>```

> [!note] `in` vs `!== undefined`
> `obj.key !== undefined` can be used to check for key existence but fails when the property exists but its value is set to `undefined`.
> ```javascript
> const testObj = { test0: undefined, test1: null };
> 
> console.log("test0" in testObj); // true
> console.log(testObj.test0 !== undefined); // false 
> console.log(testObj.test1 != undefined); // false sweet couple
> console.log("test1" in testObj); // true
>```

---
## `for..in` loops

The `for..in` loops iterates through all the keys of an object.

```javascript
const person = {
	name: "JJO",
	age: 22,
	married: false
};

for(const key in person)
	console.log(key, "->", person[key]);
```

---
## Property Ordering

Object properties are ordering in a special manner. *Integer properties* are sorted and stored in ascending order while *non-integer* properties are stored in insertion/creation order.

```javascript
let codes = {
  "49": "Germany",
  "41": "Switzerland",
  "44": "Great Britain",
  // ..,
  "1": "USA"
};

for(const code in codes) {
  console.log(code); // 1, 41, 44, 49
}
```

> [!note]
> An *integer property* is a `string` that can be converted to an from an integer without change.
> Meaning `Number(prop)` must be an integer ***and*** `String(Number(prop)) === prop`.
> ```javascript
>function isIntegerProperty(prop){
>  const isInteger = Number(prop) === Math.trunc(Number(prop));
>  const isSame = String(Number(prop)) === prop;
>  return isInteger && isSame;
>}
>
>console.log(isIntegerProperty("49")); // true
>console.log(isIntegerProperty("+49")); // false
>console.log(isIntegerProperty("1.2")); // false
> ```

#revisit is `Infinity` an integer property