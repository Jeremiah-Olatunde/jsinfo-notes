---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/function-object
banner: "![[424098.jpg]]"
---
functions are values
all values have a type
the type of functions is `object`
ergo functions can not only be called, but properties can be added or removed from them
they are also passed and compared [[object-copy]]

functions have a `name` property which stores the name of the function.
info: contextual naming
	if a function name is not provided it is assumed from the context
tip: use to find the name of a function or method regardless of where it is passed
```javascript
const foo = function(){}
const bar = foo;
function idor(diet){
  console.log(diet.name);
}
console.log(bar.name);
idor(bar);
```
this also applies to methods as well.

when the name can not be figured out from the context it defaults to an empty string

the `length` property returns the number of *parameters* of a function excluding rest parameters 
note:
	`arguments` counts the number of arguments passed in
	even if the arguments are collected into a rest parameter
	they still appear on the argument object

```javascript
function foobar(a, b, ...rest){
  console.log(arguments.length); // 5
  console.log(foobar.length); // 2
}

foobar(0, 1, 2, 3, 4);
```



custom properties can be added to functions like any object
warning:
	custom properties have no relations to local variables
	i.e. they do not become part of the environment record when a function is invoked


closures vs function properties
data stored on function properties can be accessed by external code anywhere the function is 
data stored in closures are only accessible via nested functions through the chain of lexical environment references (see [[closure]])



Named function expression
`const varName = function funcName(){}`
a named function expression is a function with a name specified 
this allows for the function expression to refer to itself, within itself regardless of it the `varName` identifier is changed

```javascript
let hello = function(){
  console.log(hello.length); // hello is taken from outer lexical environment
}
const foo = hello;
hello = null; // hello set to null in outer lexical enviroment 
foo(); // error, can't call numm
```

function declarations do not support this feature and as such have no safe way to internal refer to themselves


read later
[type introspection](https://en.wikipedia.org/wiki/Type_introspection)
[reflection](https://en.wikipedia.org/wiki/Reflective_programming)
[Polymorphism](https://en.wikipedia.org/wiki/Polymorphism_(computer_science))
