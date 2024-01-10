---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/call-apply-decorators
banner: "![[424098.jpg]]"
---
given a function that is CPU intensive but pure (outputs determined solely by its input arguments) caching the results of a calls to the function with given arguments is ideal.

this can be done with a decorator function

a decorator function takes a function as one of its argument and returns a new function responsible for calling the given function.
the decorator can wrap this call in custom logic 
the returned function is then used in place of the original 
decorator alters the behavior of the given function from the perspective of the calling code

the caching decorator function uses map 
stores the passed argument as key and the result of applying the wrapped function to that passed argument as the value
every call it looks up the key in the map. if present the value is returned 
if not the wrapped function is called and the new key value pair is inserted

note the cache is stored a closure
that is in the [[closure|environment record]] of the lexical environment of `cachingDecorator` that is referenced by the nested function's (`decorated`) hidden `[[Environment]]` property 

```javascript
function slow(x){
  for(let i = Date.now(); Date.now() - i < x;);
}

function cachingDecorator(func){
  const cache = new Map();

  return function decorated(x){
    if(cache.has(x)) return cache.get(x);

    const value = func(x);
    cache.set(x, value);
    return value;
  }
}

const decSlow = cachingDecorator(slow);

console.time("first call");
decSlow(2000);
console.timeEnd("first call");

console.time("cached call");
decSlow(2000);
console.timeEnd("cached call");
```

the decorator above fails when object methods are passed in

```javascript
const test = {
	minDelay: 1000,
	slow: function(x){
		for(let i = Date.now(); Date.now() - i < x + this.minDelay;);
	}
}

console.time("regular call");
test.slow(0);
console.timeEnd("regular call"); // 1000ms

test.slow = cachingDecorator(test.slow);; // decorating it for christmas

console.time("decorated call");
test.slow(0); // error can not read prop of undefined
console.timeEnd("decorated call"); 
```

`test.slow` is now the `decorated` function that is returned by `cachingDecorator`
`decorated` invokes the original `test.slow` method as a regular function (`func(x)`) meaning the value of `this` in `func` (i.e. `test.slow`) becomes `undefined` in [strict mode](app://obsidian.md/strict-mode) or the [global-object](app://obsidian.md/global-object) in non-strict mode. ([[object-methods#`this`|see]])
ergo when `this.minDelay` is accessed an error is thrown 

equivalent to

```javascript
const slow = test.slow;
slow(); // this === undefined 
```

`someFunc.call(thisObj, ...args)` is built-in a method on all functions
it allows for the explicit setting of the value of `this` in `someFunc` when it is invoked

```javascript
function greet(lang){
	if(lang === "en") console.log("hello my name is", this.name);
	else console.log("I only speak english");
}

const seun = { name: "jjo" };
const seni = { name: "jno" };

// this === seun
greet.call(seun, "en"); // "hello my name is jjo"

// this === seni
greet.call(seni, "en"); // "hello my name is jno"
```

replacing `func(x)` with `func.call(this, x)` resolves the error in the caching decorator

```javascript
function slow(x){
  for(let i = Date.now(); Date.now() - i < x;);
}

function cachingDecorator(func){
  const cache = new Map();

  return function decorated(x){
    if(cache.has(x)) return cache.get(x);

    const value = func.call(this, x);
    cache.set(x, value);
    return value;
  }
}

const test = {
	minDelay: 1000,
	slow: function(x){
		for(let i = Date.now(); Date.now() - i < x + this.minDelay;);
	}
}

test.slow = cachingDecorator(test.slow);; // decorating it for christmas

console.time("decorated call");
test.slow(0); // this === test
console.timeEnd("decorated call"); // 1000ms everything works
```

note:
	the `slow` property on `test` is now the function `decorated`
	`test.slow(0)` sets `this` to `test` inside `decorated`
	ergo `func.call(this, x)` calls the original `test.slow` and explicitly specifies this as `test`
	no more `undefined` this

mutli-argument
previous implementation of caching decorator only supports single argument functions
possible solutions
1. implement a multi-key map
2. use nested maps
3. use a custom hash function to generate a hash from the arguments

example:
	custom hash function could convert the arguments to strings and concatenate them

`someFunc.apply(thisObj, arguments)`
is similar to `.call` in that it allows the explicit setting of `this`
`arguments` is an [[iterable|array-like]] of the arguments to be passed in to `someFunc`
iterables will not work `someFunc({}, "hello")` errors

summary:
	`call` takes a list of arguments
	`apply` takes an array-like containing the arguments

note:
	`func.call(context, ...args)`
	`func.apply(context, args)`
	are equivalent but the `call` syntax works only for iterables `apply` only for array-likes

note:
	apply is usually more performant

method borrowing

array-like objects don't support `Array` methods.
one solution is `Array.from` see ([[iterable]]).
method borrowing is another

```javascript
// function joins the arguments array-like values into a string
function foobar(){
	// calling the join method of an array but setting this to the arguments
	// array-like object
	return [].join.call(arguments, "+"); 
}

console.log(foobar(..."hello")); // "h+e+l+l+o"
```

this works because the specification of `Array.prototype.join` works with the value of `this`
it accesses `this[0]`, ... ,`this[N]` and puts them together in a string with the specified seperator

caveats
the properties stores on decorated functions are not passed on to the return wrapper function.
proxy and reflect allows for the creation of decorators that can keep access to function properties

read later
[Decorators](https://en.wikipedia.org/wiki/Decorator_pattern)
[MDN call](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
[MDN Apply](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
