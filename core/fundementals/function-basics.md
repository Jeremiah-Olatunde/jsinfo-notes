---
tags:
  - backend
  - javascript
  - frontend
  - jsinfo
source: https://javascript.info/function-basics
banner: "![[RDT_20211230_2344588210630811137977792.jpg]]"
banner_y: 0.21715
---
# Functions
---
Functions allow for the creation of reusable blocks of code and enable adherence to the **DRY** principle. 

```javascript
// Function declaration
function funcName(param0, ..., paramN){
	// function body
	[return value]; 
}

// Function execution
funcName(arg0, ..., argN);
funcName(otherArg0, ..., otherArgN);
```

## Local and Outer Variables
---
Local variables are declared inside the body of a function. They are accessible only from the line on which they are declared to the end of the function body. Outer variables are declared outside of the body of a function but can be accessed from within the function body. If a local variable has the same name as an outer variable it ***shadows*** the outer variable making it inaccessible from within the function body. 

> [!info] Shadowing idiosyncracies
> Modifications to shadowed variables only affect the local variable and not the outer one

```javascript
const outerVar = "hello space"

function earth(){
	const localVal = "hello earth";
	const outerVar = "space has collapsed" // shadowed outer variable
}
```

> [!note] Global Variables
> Global variables are declared outside of the scope of any function and as such are available to all functions. 

## Parameters and Arguments
---
A ***parameter*** is a local variable that is specified inside the parentheses of a function declaration. An ***argument*** is the value passed into the function and assigned to the parameter which the function is called.

Since parameters are local variables they can shadow outer variables as well meaning modifications to outer variables passed as arguments to shadowing parameters do not affect the outer variable.

```javascript
function showMessage(from, text) { // parameters are from and text
  from = '*' + from + '*';
  alert( from + ': ' + text );
}

let from = "Ann";

showMessage(from, "Hello"); // outer from is shadowed by from param

// the value of "from" is the same, the function modified a local copy
alert( from ); // Ann
```

## Default Parameters
---
If a function is called but an argument for a certain parameter is not provided the value of that parameter defaults to `undefined`. Custom defaults can be provided using `param = expression` syntax. The expression is evaluated and the value assigned to `param` only if `param` is `undefined`. 

```javascript
function greetings(name, greeting = "nothing"){
	return `${name} says ${greeting}`
}

greetings("Jesuseun"); // "Jesuseun says nothing"
greetings("Jesuseun", "hello") // "Jesuseun says hello"
```

> [!note] Manually triggering default
> If a parameter is manually assigned `undefined` it is assigned the specified default
> ```javascript
> greetings("Jesuseun", undefined); // Jesuseun says nothing
>```

> [!info] Evaluation of default parameters
> The custom default expression value is evaluated every time the function is called without the respective parameter or with the parameter set to `undefined`.
> ```javascript
> function specifiedOrRandom(num = Math.floor(Math.random() * 10)){
> 	console.log(num);
> }
> 
> specifiedOrRandom(10); // 10
> specifiedOrRandom(20); // 20
> specifiedOrRandom(undefined); // 5
> specifiedOrRandom(); // 7
> specifiedOrRandom(); // 2
> ```

> [!tip]
> Use `||` or `??` to *reassign* a parameter to a default in the function body if it is not provided. 
> > [!warning] Caveats
> > `??` assigns default even if `null` is specified as the argument. ([[#[Nullish Coalescing `??`](https //javascript.info/nullish-coalescing-operator)|more info]])
> > `||` assigns default if any falsy value is specified as the argument. ([[#` `, `&&`|more info]])
>
>For example
> 
> ```javascript
> function funcA(text){
> 	text = text || "no text";
> 	return text; 
> }
> 
> function funcB(text){
> 	text = text ?? "no text";
> 	return text;
> }
> 
> function funcC(text = "no text"){
> 	return text;
> }
> 
> funcA(); // "no text"
> funcB(); // "no text"
> funcC(); // "no text"
> 
> funcA(""); // "no text" as an empty string is a falsy value
> funcB(""); // "" as "" is not null or undefined
> funcC(""); // ""
> 
> funcA(false); // "no text"
> funcB(false); // false
> funcC(false); // false
> 
> funcA(null); // "no text"
> funcB(null); // "no text"
> funcC(null); // null
>```

## `return`
---
Functions can return a value back to the calling code by placing `return returnValue;` in the body of the function. All code that comes after `return` is ignored. If a function does not use a `return` statement or the `return` is specified without a value then the function automatically returns `undefined`. 

