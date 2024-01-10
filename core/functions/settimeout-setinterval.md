---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/settimeout-setinterval
banner: "![[RDT_20211230_2341252209938260377714920.jpg]]"
---
`setTimeout` and `setInterval` are functions that allow for the scheduling of function execution
they are not part of the language specification but are instead provided by the environment 

`setTimeout(function, delay = 0, [...args]): timerId`

`function` 
	can be a string
	a function *body* is created from the string
	such conventions are depreciated (not even supported in some environments)

`delay`
	time(ms) elapsed before function invocation

`args`
	arguments to be passed into the functions

`timerID`
	an unique identifier for the timer
	in the browser it is a `number`
	in node it is a timer object

warning:
	accidentally invoking a function as opposed to passing it

`clearTimeout(timerId)`
cancel the execution of a scheduled function

`setInterval`
has the same signature as `setTimeout` but schedules the function to be run repeatedly after the specified delay has elapsed. waits delay before first call.

`clearInterval`


repeated scheduling
`setTimeout` can be used to repeatedly schedule a call.

this allows the delay time between each function call to be customized
example:
	increasing the delay between requests if a server becomes overloaded

using `setTimeout` in this was specifies the delay between the end of the previous function's execution and the next function's execution. The execution time of the function has no bearing on the ideal time between the end of the function and the start of the next.

while using `setInterval` specifies the delay between the start of the previous function execution and the start of the next function's execution.

if the function takes longer than the specified interval to run then subsequent scheduled calls occur immediately
```javascript

let marker = Date.now();

function delay(ms){
  console.log(Date.now() - marker, "Delaying for", ms, "ms");
  for(let start = Date.now(); Date.now() - start < ms;);
  console.log(Date.now() - marker, "end of delay");
}

// const x = setInterval(() => delay(1000), 2000);
const x = setInterval(() => delay(2000), 1000);
setTimeout(() => clearInterval(x), 7000);
```

note: garbage collection

zero delay `setTimeout`
calling `setTimeout` with the delay set to `0` does not cause the function to be invoked immediately
instead it is places it on the scheduler. functions in the scheduler are only run when the currently executing script is done
example

info
	after 5 nested timers the interval is forced to be at least 4ms according to the read later [HTML Living Standard](https://html.spec.whatwg.org/multipage/introduction.html#introduction)
	non browser environments do not have such limitation


