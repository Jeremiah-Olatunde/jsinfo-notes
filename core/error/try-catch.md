---
tags:
  - backend
  - frontend
  - javascript
  - draft
  - jsinfo
source: https://javascript.info/try-catch
---
The `try...catch` construct is used to handle errors. 
the code in the `try` block is executed.  
if the script encounters an error then the execution of the `try` block is halted and the control flow immediately jumps to the catch block.  
the error object is passed into the catch block.  

```javascript
try {
  // code
} catch(error){
  // error handling code
} finally {

}
```

> [!info]
> variables defined in the block scopes of `try`, `catch` and `finally` are local to their blocks

> [!warning]
> `try...catch` only works for run-time errors i.e. exceptions 
> syntax errors are not caught as the engine can not parse and understand them

> [!note]
> `try...catch` works synchronously. If a function scheduled inside a `try` block throws the exception will not be caught by the `catch` block.

> [!tip]
> If the error object is unused the parentheses after `catch` can be omitted 

error object
the error object contains information about an error that has occurred
the main properties are
- `name`: the name of the error eg. `ReferenceError`
- `message`: text description of the error

different environments support other properties  
one of the most commonly supported is `stack`.  
`stack` is a string of information about the sequence of nested calls that let to the error

there exist a number of built-in error object each with its corresponding constructor.  
these error constructors take the `message` as their only argument.  
the `name` property of the error is exactly the same as the name of the constructor.   

```javascript
new Error(message);
new SyntaxError(message);
new ReferenceError(message);
```

```javascript
const error = new EvalError("could not evaluate this drip");

console.log(error.name === error.__proto__.constructor.name); // true
console.log(error.message); // "could not evaluate this drip"
```

`throw value` keyword allows for manual error throwing
`value` maybe any JavaScript value but is usually an error object

```javascript
throw Symbol("hello world");
throw "unimplemented";
throw 5;
throw new Error("insufficient funds (sapa wins again)");
```

> [!example]
> throwing an error if a property is missing on a `JSON` parsed object

> [!tip]
> Ideally try to use built-in errors that make semantic sense for the situation

rethrowing
`try...catch` blocks catch all errors that execute inside the `try` block
best practice is to handle only a specific error and rethrow any other error
this is done by assessing the error object in the `catch` block;
- `error instanceof TargetError`
- `error.name === "TargetErrorName"`
if the error object is not the target error then `throw` is used to rethrow the error

```javascript
class NoNameError extends Error {}

function getName(){
  try {
    // JSON.parse throws SyntaxError is invalid JSON is passed
    const json = JSON.parse(`{"bar": "Jesuseun Jeremiah Olatunde"}`);
  
    if(json.name === undefined) throw new NoNameError("name not defined");
  
    return json.name; // returns if JSON valid and name prop
    
  } catch(error) {
  
    if(error instanceof SyntaxError) {
      // print if invalid JSON recieved
      console.log("Our serves were not able to process that request");
    } else if(error instanceof NoNameError) {
      // print if no name property on JSON object
      console.log("Seems like the user signed up with invalid data");
    } else {
      // if neither of the target errors above, rethrow
      throw error;
    }
  }
}

const name = getName();
```

finally

`finally` runs after the execution of the `try...catch` block regardless of the error state.
it is typically used to run code that should be executed whether or not an error occurs.
this could be clean up code eg. closing  database connection
this could be measuring code e.g. benchmarking a function that might error.

> [!note]
> `finally` runs on any and all exits from the `try..catch` block including when either block `return` s

> [!tip]
> It is possible to use just the `try...finally` blocks.  
> This is typically done when errors are not intended to be handled but some finalization is code should always be run


global error handling

JavaScript environments provide mechanisms to catch and listen to errors globally  
Browsers provide `window.onerror` property.  

`window.onerror = function(message, url, line, col, error){}`

when an error occurs in the script the function assigned to `onerror` is called with the above arguments.  
this is typically used for things like sending the error to some logging service.


> [!note]
> `window.onerror` can not be used for error recovery as the error is still thrown and the script dies. It is solely used to execute code before the script dies
> ```javascript
> window.onerror = function(message){
>   console.log(message);
> }
> 
> falalalala;
> ```

