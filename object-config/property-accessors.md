---
tags:
  - backend
  - frontend
  - draft
  - javascript
  - jsinfo
source: https://javascript.info/property-accessors
banner: "![[RDT_20211230_2342055806120662248886414.jpg]]"
---
There are 2 kinds of properties
1. data properties: regular properties
2. accessor properties: functions that execute on getting/setting a value but are interacted with like regular properties 

accessor properties are representing by getter and setter methods 
specified using the method short-hand syntax  and placing `get` or `set` keywords before the method name.

```javascript
const user = {
	firstName: "Jesuseun",
	lastName: "Olatunde",

	get fullName(){
		return this.firstName + " " + this.lastName;
	},
	
	set fullName(value){
		[this.firstName, this.lastName] = value.split(" ");
	}
}

console.log(user.fullName); // Jesuseun Olatunde
user.fullName = "Lorem Ipsum";
console.log(user.fullName)l // Lorem Ipsum
```

`fullName` is an accessor property with only a getter specified.
note:
	`fullName` is accessed as a regular property as opposed to being invoked as a method

warning:
	arrow functions and function literals (function properties) do not work (syntax error)

warning:
	In strict mode an error is thrown if an accessor property without the setter is modified
	in non-strict mode the modification silently fails

note:
	the assignment operator returns the assigned value independently of the setter

warning:
	if the getter is missing accessing the property returns `undefined` regardless or mode

Accessor descriptors

accessor descriptors have `get` and `set` properties in place of  `value` and `writable`

```javascript
const user = {
	firstName: "Jesuseun",
	lastName: "Olatunde",
}

Object.defineProperty(user, "fullName", {
  get: function(){ 
    console.log("getting full name");
    return this.firstName + " " + this.lastName; 
  },

  set: function(value){
    console.log("setting full name");
    [this.firstName, this.lastName] = value.split(" ");
  },

});

console.log(user.fullName);
user.fullName = "Lorem Ipsum";

// configurable: false (default value)
// enumerable: false (default value)
```

note: 
	omitting `set` is equivalent to `writable: false`

note:
	arrow functions can be used when defining accessor properties using the descriptor method
	all caveats of arrow functions apply 

warning:
	specifying both `value`/`writable` and `get`/`set` throws an error

smart accessors 
accessor can be used to wrap hidden properties an implement custom logic that is run whenever the hidden property is accessed. e.t.c validation
hidden properties use the `_propNmae` naming convention.

data properties can be converted to accessor properties to implement desired custom logic updates without affecting external code