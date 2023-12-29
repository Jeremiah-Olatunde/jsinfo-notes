---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/object-copy
banner: "![[RDT_20211231_0544028205562906102267974.jpg]]"
banner_y: -10
---
# Object references and copying

When an object is assigned to a variable the variable stores a copy of the address of the object in memory(a.k.a a reference to the object). It is this address, and not the object itself, that is duplicated when an object is copied from one variable (or parameter, or property) to another. This manner of copying is referred to as ***copying by reference***.

---
# Copying by reference

When an object is copied from one variable to another the address stored in the variable is copied not the object itself.  The original variable and the variable the object was copied into both store the address of the same underlying object.

```javascript
const user = { name: "John" }; // user stores a memory address
const admin = user; // admin now stores a copy of that memory address
```

When an object stored in a variable is interacted with (i.e. a property is accessed, modified or deleted) the engine dereferences the address, accessing the real object at the specified location in memory, and performs the operation on the object. Ergo if two variables store the same address (i.e. reference the same object in memory) any modification made to the object stored at that address reflects in both variables.

```javascript
const user = { name: "John" };
const admin = user;

admin.name += admin.name; // modifying the object through the admin variable

console.log(user.name); // user becomes a firebending master
```

> [!note] 
> Properties of objects that are themselves objects are no different in that they store the address in memory of the object they reference. This means that when they are copied it is this address that is copied as well
> ```javascript
> const user = {
> 	// the name object property stores the address of the object
> 	name: {
> 		firstName: "Jesuseun",
> 		lastName: "Olatunde",
> 	},
> 	age: 22,
> };
> 
> const name = user.name; // assigning the address to the variable name
> 
> const userFromTheFuture = {
> 	// assigning the address in the name variable to the name property 
> 	// using the shorthand syntax
> 	name, 
> 	age: 52,
> };
> 
> // modifying the object through the name variable
>name.lastName = name.lastName.split("").map(x => x.toUpperCase())
> 
> // the modification reflect through all variables an properties whose values
> // were references to the modified object
>console.log(user.name.lastName);
>console.log(name.lastName);
>console.log(userFromTheFuture.name.lastName);
>```

> [!info] `const` object properties are mutable
> - Objects assigned to `const` variables can be modified. Why? because creating a variable with `const` guarantees that its ***value*** will never be changed. 
> - When an object is assigned to a variable the ***value*** of the variable is the object's address in memory not the object itself
> - Modifying the object does not change its address in memory, therefore an object can be modified while leaving the ***value*** of the variable unchanged.
> ```javascript
> const person = { name: "Jesuseun" };
> const other = { name: "Jack" };
>  // no error because the value of person (the address in memory of the object) 
>  // remains unchanged
>  
> person.name = "Olatunde";
> person = other 
> // fails on attempt to change the value of person 
> // to the address of another object
>```

---
# Comparison by reference

objects are compared by reference too.  Two variables referencing an object are equal only if they reference the same object i.e. they both contain the same memory address value. If two variables store objects that are identical in every way but are stored at different memory locations and hence are different entities then they are not equal.

```javascript
const person = { name: "Jesuseun" };
const shadowCloneJitsu = person;
const samePerson = { name: "Jesuseun" };

console.log(person === shadowCloneJitsu) // true
console.log(person === samePerson) // false
```

---
# Object cloning

## Shallow Copies

Object clones can be made by iterating over the properties of the object and cloning them on the ***primitive*** level. However if the value of a property is itself and object (i.e. the value is the memory address of an object) it is copied by reference as demonstrated earlier. Such copies are called shallow copies

```javascript
function shallowCopy(obj){
  const copy = {}; // create empty object
  for(const key in obj) // iterate through all keys including symbol keys
    copy[key] = obj[key];
  return copy;
}

const person = {
  name: {
    first: "Jesuseun",
    middle: "Jeremiah",
    last: "Olatunde",
  },
  age: 22,
  married: false,
  greet: function(){
    return `hello I am ${this.name.first}`
  }
}

const copy = shallowCopy(person);

// object properties of person such as name and greet are copied by reference
console.log(copy.name === person.name); // true
console.log(copy.greet === person.greet); // true
```

[`Object.assign(destObj, ...sourceObjs)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) can also be used to make shallow clones as opposed to manually iterating over the properties. Properties in the target object are over written by properties in the sources. Later sources' properties overwrite earlier ones.

```javascript
const copy = Object.assign({}, person, { name: "Jesuseun Jeremiah Olatunde" });
console.log(copy.name === person.name); // false name property over written
console.log(copy.greet === person.greet); // true shallow copy
```

---
## Deep copies

In order to create a true copy object properties themselves must be cloned as well. This can be done using recursion. When iterating over the keys of the object the `typeof` can be used to check if the value is an object (`typeof obj === "object"`) or a primitive. If the value is a primitive it can be copied normally however if it is an object then the cloning function is called on that object.

```javascript
function deepCopy(obj){
  const copy = {};
  for(const key in obj)
    copy[key] = typeof obj[key] == "object" ? deepCopy(obj[key]) : obj[key];
  return copy;
}

const deep = deepCopy(person); // that's what she said

console.log(deep.greet === person.greet); // true greet method isn't deep cloned
console.log(deep.name === person.name); // false name object is deep cloned

person.myself = person; // circular reference
deepCopy(person); // stack overflow error
```

> [!warning] `typeof someFunction !== "object"`
> The implementation of deep cloning above fails to copy function properties because apply `typeof`  to a function returns `"function"` not `"object"`. (see [[types##The `typeof` Operator]])

> [!warning] Circular references and infinite recursions
> The implementation of deep cloning also fails when an object has a circular reference i.e. property that references itself or an object in the chain of recursive calls

---
## `structuredClone`

Deep copies can also be made by using the global function [`structuredClone(object)`](https://developer.mozilla.org/en-US/docs/Web/API/structuredClone).
`structuredClone` creates a deep clone of `object` using the structured clone algorithm. Unlike the implementation of `deepClone`, `structuredClone` is capable of handling circular references. 

```javascript
const deep = structuredClone(person); 
person.myself = person; // circular reference

console.log(deep.name === person.name) // false
console.log(person.myself === deep.myself) // false 

```

> [!warning] No function properties either
>`structuredClone` can not deep copy function properties and throws a `DataCloneError`.

---
# Further Reading

1. [`Object.assign(destObj, ...sourceObjs)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
2. [`structuredClone(object)`](https://developer.mozilla.org/en-US/docs/Web/API/structuredClone)
