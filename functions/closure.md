---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/closure
banner: "![[RDT_20211230_2338352427645445258369028.jpg]]"
---
code blocks `{}`
a variable declared in a code block is only accessible from within that code block.
`if`, `for` and `while` blocks have this property as well.
variables declared in the parentheses of the `for` statement are considered to be within its code block

functions can easily be nested in JavaScript
functions are values hence nested functions can be returned as is or as properties like any other value

Lexical Environment
Every ***running*** function, code block and the script itself  have associated with them a hidden internal object called the *Lexical Environment*. This is analogous to the scope i.e function-scope, block-scope, global-scope
*global Lexical Environment* is associated with the whole script


the lexical environment is made up of:
- *Environment Record*: an object that stores all local variables as properties (and other information)
- A reference to the outer lexical environment

variables
A variable is just a property of the *Environment Record*, accessing or mutating a variable equivocates to accessing or getting a property of the environment record

1. when the execution of a lexical environment starts the environment record is prepopulated with all declared variables. These are set to an `unintialized` state indicating the can not be accessed.
2. when the execution hits a `let` or `const` declaration of a variable becomes it `undefined`
3. when the execution hits the variable assignment the variable is assigned its value
note: this applies to code block, global, function
examples:
```javascript
let x = 1;

function func() {
  console.log(x); // ReferenceError: Cannot access 'x' before initialization
  let x = 2;
}

func();
```
note: 
	variable declaration and assignment usually occurs on a single line `const x = 5;`

function
function are values.
unlike variables functions created using function declaration are fully initialized and their values assigned at the start of execution. 
hoisting


inner and outer lexical environments
At definition the lexical environment in which a function is defined is stored on a special internal property called `[[Environment]]`. 
when a function is ***invoked*** a new lexical environment is created 
this environment stores the local variables and parameters in its environment record 
the environment references the outer lexical environment stored in the functions `[[Environment]]` property.
note:
	a new lexical environment is created for each invocation of the same function. 
	example:
		multiple `makeCounter` calls. ^5759ac

When a variable is accessed, the chain of lexical environments are searched beginning from the inner (current) lexical environment down to the global lexical environment.
if the variable is not found an error is thrown in strict mode.
variables are accessed from and mutated on the environment record of the corresponding lexical environment in which they are stored
example:
	`makeCounter` ^af12ed

all lexical environments down the chain of references will *"see"* variable mutations. ^x
example:
	return multiple functions ^262ec9

[closure](https://en.wikipedia.org/wiki/Closure_(computer_programming))


garbage collection
lexical environments (like any object) are removed from memory by the garbage collector when all references to them are erased and they are no longer reachable. 
when a function returns its lexical environment is collected by the garbage man
however
if a function returns a nested function `[[Environment]]` property of that nested function references the lexical environment of the returning function
ergo the lexical environment is not collected even after the function returns

engine optimizations
in theory when a function is alive all outer variables are kept alive as well
however in practice many engines optimize the process and delete any outer variables that are obviously not referenced.
this can have strange impacts on debugging

read later
[random number generator](https://en.wikipedia.org/wiki/Pseudorandom_number_generator)


tasks
#coding-problems/javascript/jsinfo 

The function sayHi uses an external variable name. When the function runs, which value is it going to use? (see [[#^262ec9]])
```javascript
let name = "John";

function sayHi() {
  alert("Hi, " + name);
}

name = "Pete";

sayHi(); 
// "Pete" 
```

what will it show? (see [[#^af12ed]])
```javascript
function makeWorker() {
  let name = "Pete";

  return function() {
    alert(name);
  };
}

let name = "John";

// create a function
let work = makeWorker();

// call it
work();
// "Pete" searches chain of lexical environment (work -> makeWorker -> gobal)
// hits makeWorker first
```

are the counters independents (see [[#^5759ac]])
```javascript
function makeCounter() {
  let count = 0;

  return function() {
    return count++;
  };
}

let counter = makeCounter(); // makes a new lexical environment (makeCounter0)
let counter2 = makeCounter(); // makes a new lexical environment (makeCounter1)

alert( counter() ); // 0 uses makeCounter0
alert( counter() ); // 1 uses makeCounter0

alert( counter2() ); // 0 uses makeCounter1
alert( counter2() ); // 1 uses makeCounter1
```

counter object, what will it show? (see [[#^262ec9]])

```javascript
function Counter() {
  let count = 0;

  this.up = function() {
    return ++count;
  };
  this.down = function() {
    return --count;
  };
}

let counter = new Counter(); // single lexical environment created 

// all methods access that lexical environment
// changes to environment record properties are seen by all functions referencing it
alert( counter.up() ); // 1
alert( counter.up() ); // 2
alert( counter.down() ); // 1
```

sum with closures

```javascript

sum(1)(2) = 3
sum(5)(-1) = 4
```