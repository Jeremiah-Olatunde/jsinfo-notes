---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/switch
banner: "![[Sunset.png]]"
---
# The `switch` statement
---
The `switch` statement offers a more descriptive and concise way to compare a value with multiple possible variants such a variable storing the day of the week which has 7 variants.

The `switch` statements checks its arguments against the values of the expression variants specified in the case blocks for **[[#Strict Equality (`===`, `!==`)|strict equality]]**. If a match is found, the code of the matching case block ***down to the nearest case block with a `break` statement*** are executed. If no match is found the `default` case (if specified) is executed.

```javascript
switch(argument){
	case expression0:
		// execute if expression0 === argument
		[break] // optional break
	case expresssion1:
		// execute if expression1 === argument
		// or if expression0 === argument and the break statement in expression0
		// code block is omitted
		[break]
	case expressionN:
		// execute if expressionN === argument
		// or if expressionM (0 <= M < N) === argument and the break statement from
		// expressionM to expressionN-1 is omitted
		[break]
	default:
		// execute if none of the expressions === argument
		[break]
}
```

> [!note] Type Matters
> The `switch` statement checks for strict equality

> [!tip] Case Grouping
> Cases can be grouped by omitting `break` statements
> ```javascript
> const toMatch = Math.floor(Math.random() * 11);
> switch(toMatch){
> 	case 0:
> 	case 1:
> 	case 2:
> 	case 3:
> 	case 4:
> 		console.log("random number between 0 and 4 inclusive");
> 		break;
> 	case 5:
> 	case 6:
> 	case 7:
> 	case 8:
> 	case 9:
> 		console.log("random number between 5 and 9 inclusive");
> 		break;
> 	default:
> 		console.log("random number is 10");
> 		break;
> }
>```