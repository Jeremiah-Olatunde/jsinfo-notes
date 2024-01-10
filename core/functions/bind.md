---
source: https://javascript.info/bind
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
banner: "![[RDT_20211230_2340276559390054174659129.jpg]]"
---
when a method is passed somewhere separately from the object and invoked without the context of that object `this` is lost

example:
	assigning a method to a regular variable and calling the variable

note:
	- `setTimeout` sets `this` to `Window` when invoking its callback (in browser)
	- NodeJS sets `this` to the timer object


solution 1
using a wrapper functions when passing methods into `setTimeout` or `setInterval`
`this` is the object before the dot as always

```javascript
const user = { 
	name: "Jesuseun",
	greet: function(){ console.log("Hello from"m this.name); },
};
setTimeout(() => user.greet(), 1000);
```

the value of user can be reassigned during the time delay leading to unintended behavior.
example:
	change the `user` object to an `admin` object
note: 
	this is not a problem when using the `const` assignment keyword

solution 2
`Function.prototype.bind` can be used to permanently fix the `this` value for a function.
it returns a new function with the fixed value of `this`

```javascript
const bound = func.bind(thisArg, ...boundArgs);
```

example

```javascript
const greet = function(){ 
	console.log("Good morning from", this.name); 
};
const user = { name: "Jesuseun" };


const bound = greet.bind(user);
bound(); // Good morning from Jesuseun
greet(); 
// Good morning from undefined (in non-strict mode)
// can not read property of undefined error in strict mode
```

warning:
	arrow functions can not be bound
	bound arrow functions still take this from the outer lexical environment


using bind to solve `setTimeout` problems

```javascript
const user = { 
	name: "Jesuseun",
	greet: function(){ console.log("Hello from"m this.name); },
};
setTimeout(user.greet.bind(user), 1000);
```


when the new function is invoked the `boundArgs` are passing in first and then any other argument passed into the new bound functions

```javascript
function foobar(...args){
  console.log(...args);
}

const bound = foobar.bind(null, 0, 1, 2);
bound(3, 4, 5, 6); // 0 1 2 3 4 5 6
```

this can be used to implement partial function application
partial functions are functions with some of their arguments pre-applied 
this differs from Haskell where a [ partial function](https://wiki.haskell.org/Partial_functions) is one whose behavior is not defined over the range of its possible inputs 


example:
	partial function application to create a `double` function from a `multiply` function

using bound in implement partial function application forces the permanent fixing of this
this makes implementing partial application for methods tricky
use a method similar to the caching decorator to implement a function that circumvents this

```javascript
function partial(func, ...boundArgs){
  return function dummy(...remArgs){
    func.call(this, ...boundArgs, ...remArgs);
  }
}

const user = {
  name: "lorem ispum",
  greet: function(...args){ console.log(this.name, ...args); }
}

user.greet = partial(user.greet, 0, 1, 2); // user.greet is now the "dummy" function
user.greet(4, 5, 6); // this in dummy is user as user is the object before the .
// there fore invoking the original user.greet with call(this, ...) sets this to user
// pass in args
```