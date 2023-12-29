---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/function-expressions
banner: "![[Ronin-Swing_1-72.jpg]]"
---
# Function expressions
---
JavaScript allows function to be created via an expression. Since expressions always evaluate to a value this implies that JavaScript functions are values. Therefore the resulting function defined using the function expression syntax can be treated like any other value in JavaScript i.e. assigned to a variable, passed as an argument into another function and so on. This is also true for functions declared using the function declaration syntax.

```javascript
// Function declaration
function fib(num){
	if(num == 0 || num == 1) return num;
	else return fib(num - 1) + fib(num - 2);
}

// Function expression (no name after function keyword)
const fib = function(num){
	if(num == 0 || num == 1) return num;
	else return fib(num - 1) + fib(num - 2);
};
```

> [!note] Semicolons
> - Function expressions in assignment statements (such as `fib`) end with a `;` as all statements end in semicolons.
> - Function declarations are not statements hence they do not end in a `;`

## She never Callback
---
Functions are *first class* citizens in JavaScript. They are treated just like any other value and as such can be passed as argument into another function just like `number` or `string`. When a function is passed as an argument into another function the passed function is said to be a *callback function*. The calling function can then arbitrarily invoke the callback at anytime.

```javascript
function fib(num){
	if(num == 0 || num == 1) return num;
	else return fib(num - 1) + fib(num - 2);
}

function callFunc(func, arg){
	func(arg);
}

callFunc(fib, 10); // passing fib as a value to another function
```

## Hoisting
---
Function expressions are created when the execute reaches the statement and are usable only from that moment onwards. Function Declarations are available anywhere within the scope in which it is defined including prior to the line on which it is declared, this is known as [***hoisting***](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting). 
Before running a script JavaScript engines looks for all global function definitions and initializes them making them available immediately the script is actually run.

```javascript
factorial(10); // The factorial function is available before declaration

function factorial(n){
	if(n === 0) return 1;
	else return n * factorial(n - 1);
}
```

> [!warning] Always `"use strict"`
> If strict mode is enabled functions are only available within the code block they are defined.
> ```javascript
> factorial(10); // The factorial function is not available here in strict mode
> 
>if(true){
>	factorial(10); // The factorial function is available here
>	
>	function factorial(n){
>		if(n === 0) return 1;
>		else return n * factorial(n - 1);
> 	}
> 	
>	factorial(9);// factorial function is available here too
>}
>```

# Further Reading
---
- [ ] [Hoisting](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting) Mozilla Developer Network.  