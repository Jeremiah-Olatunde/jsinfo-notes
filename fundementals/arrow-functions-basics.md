---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/arrow-function-basics
banner: "![[Full moon.jpeg]]"
banner_y: 0
---
# Arrow functions, the basics
---
A simple and concise syntax for creating functions. 

```javascript

// no parameter
const func = () => expression; // implicity returns expression value

// single paremeter
const func = arg => expression; // implicity returns expression value

// multi-parameter
const func = (arg0, ..., argN) => expression; // implicity returns expression value
 
// multiline 
const func = (arg0, ..., argN) => {
	// multistatement code block here;
	return someValue; // explicit return statement required
}
```

> [!example]
> Examples of multiple arrow function variants
> 
> ```javascript
> // tail recursive factorial
> const fact = (n, accum = 1) => n ? fact(n - 1, accum * n) : accum;
> 
> // terrible code quality fibonacci
> const fib = n => (!n || n == 1) ? n : fib(n - 1) + fib(n - 2) 
>```

