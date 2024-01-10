---
tags:
  - backend
  - frontend
  - draft
  - javascript
  - jsinfo
source: https://javascript.info/extend-natives
---
Built-in classes `Array`, `Map` are extensible

```javascript
class PowerArray extends Array {
	isEmpty(){
		// length inherited from Array.prototype
		return this.length === 0;
	}
}

// PowerArray has no constructor so Array constructor is used
let arr = new PowerArray(1, 2, 5, 10, 50);

// custom method
console.log(arr.isEmpty); // false

// filter inherited from Array.prototype
console.log(arr.filter(x => x < 10)); // [1, 2, 5]
```


note:
the `filter`, `map` etc. use the `constructor` property to create a new object to which the elements are added.
recall `F.prototype` has an automatically added `constructor` referencing `F`
this applies to classes as well
hence `filter`/`map` return new `PowerArray`s 

```javascript
class PowerArray extends Array {
	isEmpty(){
		// length inherited from Array.prototype
		return this.length === 0;
	}
}

// PowerArray has no constructor so Array constructor is used
let arr = new PowerArray(1, 2, 5, 10, 50);

console.log(PowerArray.prototype.constructor); // PowerArray
console.log(arr.constructor); // PowerArray
console.log(arr.filter(x => x < 10).isEmpty()); // false

arr.constructor = Array;
console.log(arr.filter(x => x < 10).isEmpty()); // Error no isEmpty on Array
```

`Symbol.species` is a special static getter function that can be added to a class.
it returns the constructor that should be used to make new instances of a class internally
note if note specified as a static getter the `constructor` property is used as normal

```javascript
class PowerArray extends Array {
	isEmpty(){
		// length inherited from Array.prototype
		return this.length === 0;
	}
	static get [Symbol.species](){
		return Array;
	}
}

// PowerArray has no constructor so Array constructor is used
let arr = new PowerArray(1, 2, 5, 10, 50);
console.log(arr.filter(x => x < 10).isEmpty()); // Error no isEmpty on Array
```

interesting aside
the algorithm the `filter`/`map` methods use does not require an instance of Array
- create new object using `[Symbol.species]` or `arr.constructor`
- use `newObject[i]` to add the generated elements
this means that any object can be used.

```javascript
class Boom {}

class PowerArray extends Array {
	isEmpty(){
		// length inherited from Array.prototype
		return this.length === 0;
	}

  static get [Symbol.species](){
    return Boom
  }
}

// PowerArray has no constructor so Array constructor is used
let arr = new PowerArray(1, 2, 5, 10, 50);
console.log(arr.filter(x => x < 10)); // Boom { '0': 1, '1': 2, '2': 5 }
```

note:
Built-ins aren't technically classes, they are functions. (function constructors)
hence functions can be extended using class syntax

```javascript
class TrueClass {}

console.log(Array); // [Function: Array]
console.log(TrueClass); // [class TrueClass]
```

this has certain implications
The inheritance of built-ins is accomplished using regular prototype mechanics
e.g. manually setting `Array.prototype.[[Prototype]]` to reference `Object.prototype`
as opposed to using `Array extends Object {}`

this means that there is no static inheritance between built-in
recall `extends` not only sets `Child.prototype.[[Prototype]]` to `Parent.prototype` but it also sets `Child.[[Prototype]]` to `Parent` allowing `Parent`'s static properties to be inherited by child.

```javascript
class Custom extends Object {}

// custom inherits Object static properties
console.log(Custom.keys);
console.log(Custom.values);

// Array does not inherit Object static properties
console.log(Array.keys); // undefined
console.log(Array.values); // undefined
```

