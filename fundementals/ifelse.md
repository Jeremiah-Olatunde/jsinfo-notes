---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/ifelse
banner: "![[RDT_20211231_0542537504318632100686665.jpg]]"
---
# Conditional branching: `if`, `?`
---
Conditional statements allow for the execution of code blocks based on the truthiness of a certain condition.

## `if`-`else`
---

```javascript
if(expression0){
	// run code block if expression0 is true
} else if(expression1){
	// run if expression0 is false but expression1 is true
} else if(expression2){
	// run if expression0, expression1 are false but expression2 is true
} else if(expressionN){
	// run if expression0 to expressionN-1 are false but expressionN is true
} else {
	// run if all expressions are false
}
```

> [!note]
> The resulting value of the `expression` in the conditional statement is converted to a `boolean`type. ([[#Boolean Conversion]])

## Ternary Operator `?`
---
The `?` operator returns a value based on the truthiness of the `expression` provided to it.
If `expression` is truthy then the first value is return, otherwise the second value is returned. This allows for elegant assignments and function returns.

```javascript
const varName = expression ? valueIfTrue : valueIfFalse;

function myFunc(){
	return expression ? valueIfTrue : valueIfFalse;
}
```

> [!note]
> The `?` operator has a relatively low precedence and as such doesn't require the use of `()` in *most* cases

> [!example]
> The `?` operator can be chained
> ```javascript
> const varName = expression0  ? valueIfExpression0 
> 							 : expression1 ? valueIfExpression1
>                                            : valueIfFalse
>
> ```

