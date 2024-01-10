---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/logical-operators
banner: "![[RDT_20211230_2341252209938260377714920.jpg]]"
---
# Logical operators
---
There are four logical operators in JavaScript. The AND (`&&`),  OR (`||`),  NOT (`!`) and Nullish Coalescing (`??`).

## `||`, `&&`
---
`a || b` evaluated left to right and returns the value of the first truthy operand or the last value if neither of the operands are truthy. In contrast `a && b` returns the first falsy value of the last value if neither of the operands are falsy. In both cases both operands are converted to `boolean` type before the operator is applied.

>[!example]
>Get the first truthy value from a list of expressions or variables
>```javascript
>function name(firstName, lastName, nickName){
>	return firstName || lastName || nickName || "Anonymous";
>}
>name("", "", ""); // returns anonymous
>name("", "Olatunde", "JJO"); // returns "Olatunde"
>```
>Using the `||` and `&&` for short circuit evaluation
>```javascript
>// run expression if left expression is false
>Math.random() < 0.5 || expression 
>
>// run expression if left expression is true
>Math.random() < 0.5 && expression
>```

> [!note]
> - The precedence of `&&` is higher than `||`
> - Modify-in-place logical operators `&&=` and `||=`

## `!`
---
`!` is the `boolean`  NOT operator. It is a unary operator meaning it operates only on one operand. `!value` returns the `boolean` inverse of `value`. i.e. `true` of `value` is falsy and `false` if value is truthy.

> [!tip]
> - `!!value` can be used to convert a `value` to `boolean` type thereby acting as the shorthand to `Boolean(value)`.  
> - The first `!` converts `value` to `boolean` then inverts it while the second `!` undoes the inversion leaving the original `boolean` form of `value`

> [!warning]
> The logical not does not support the modify-in-place syntax as the syntax designated to not equals operator (`!=`)
## Tasks
---
#coding-problems/javascript/jsinfo 

What are the outputs of the code below

```javascript
alert(null || 2 || undefined)    
// displays 2 (|| returns the first truthy)

alert(alert(1) || 2 || alert(3)) 
// displays 1 (alert returns undefined which is falsy)
// 2 is truthy shortcircuiting the evaluation and returning 2
// the returned 2 is displayed by the outter alert

alert( 1 && null && 2 );
// displays null (&& returns the first falsy value)

alert(alert(1) && alert(2));
// displays 1 then undefined

alert( null || 2 && 3 || 4 );
// 2 && 3 returns 3 as neither of the operands are falsy and 3 is the last value
// 3 is then returned as it is the first truthy value
```



# [Loop The Loop](https://javascript.info/while-for)
---
Loops provide a mechanism to execute a certain code block for a specified number of iterations. JavaScript provides many loop structures such as `while`, `do..while` and the `for` loops.

All loops consisted of a condition, used to determine if the next iteration of the loop should be run, a [sentinel](https://en.wikipedia.org/wiki/Sentinel_value) which uses its presence as a condition for termination and a body which contains the code to be executed repeatedly. 

## Syntax
---

In a `while loop` and a `do..while` loop the sentinel is typically specified outside of the loop construct.

> [!tip]
> JavaScript allows the omission of `{}` around code blocks if they exist on a single line

```javascript
let sentinel = value;
while(conditionInvoldingSentinel){
	// code block is executed repeatedly while condition evaluates to true
	expressionToModifySentinel;
}
```

```javascript
let sentinel = value;
// The do..while loop executes the code block first before checking the condition
do {
	// code block is executed repeatedly while condition evaluates to true
	expressionToModifySentinel;
} while(condition);
```

In a for loop the sentinel is typically initialized within the `begin` expression` and modified in the `step` expression.

```javascript
for(begin; condition; step){
	// code block is executed repeatedly while condition evaluates to true
}

// the begin and step expressions can be skipped if the sentinel is initialized and modified outside the loop
for(; condition;){

}

// the condition expression can be omitted to create an everlsting loop 
for(begin;; step){

}
```

## `break` and `continue`
---
The `break` keyword can be used to exist a loop early. The `continue` keyword is used to skip the rest of the code body and continue from the next iteration. `break` and `continue` are typically combined with [[#[Conditional Branching](https //javascript.info/ifelse)|conditional branching]] in order to exit from a loop or skip an iteration conditionally.

> [!info]
>  `break` can be used inside any code block while `continue` can only be used inside loops

> [!warning]
> `break` and `continue` can not be used inside of the `?` operator.

Loops can be labeled to allow for more fine grained control when using `break` and `continue` in nested loops. Once labelled code can conditionally `break` or `continue` to the labelled loop by specifying the label after the keywords i.e. `break label` or `continue label`.

> [!example]
> Loop label syntax:
> ```javascript
> labelNameOuter: for(;;){
> 	// outer loop code body
> 	labelNameInner: for(;;){
> 		if(condition) break labelNameOuter 
> 	}
> }
>```

# [Loop The Loop](https://javascript.info/while-for)
---
Loops provide a mechanism to execute a certain code block for a specified number of iterations. JavaScript provides many loop structures such as `while`, `do..while` and the `for` loops.

All loops consisted of a condition, used to determine if the next iteration of the loop should be run, a [sentinel](https://en.wikipedia.org/wiki/Sentinel_value) which uses its presence as a condition for termination and a body which contains the code to be executed repeatedly. 

## Syntax
---

In a `while loop` and a `do..while` loop the sentinel is typically specified outside of the loop construct.

> [!tip]
> JavaScript allows the omission of `{}` around code blocks if they exist on a single line

```javascript
let sentinel = value;
while(conditionInvoldingSentinel){
	// code block is executed repeatedly while condition evaluates to true
	expressionToModifySentinel;
}
```

```javascript
let sentinel = value;
// The do..while loop executes the code block first before checking the condition
do {
	// code block is executed repeatedly while condition evaluates to true
	expressionToModifySentinel;
} while(condition);
```

In a for loop the sentinel is typically initialized within the `begin` expression` and modified in the `step` expression.

```javascript
for(begin; condition; step){
	// code block is executed repeatedly while condition evaluates to true
}

// the begin and step expressions can be skipped if the sentinel is initialized and modified outside the loop
for(; condition;){

}

// the condition expression can be omitted to create an everlsting loop 
for(begin;; step){

}
```

## `break` and `continue`
---
The `break` keyword can be used to exist a loop early. The `continue` keyword is used to skip the rest of the code body and continue from the next iteration. `break` and `continue` are typically combined with [[#[Conditional Branching](https //javascript.info/ifelse)|conditional branching]] in order to exit from a loop or skip an iteration conditionally.

> [!info]
>  `break` can be used inside any code block while `continue` can only be used inside loops

> [!warning]
> `break` and `continue` can not be used inside of the `?` operator.

Loops can be labeled to allow for more fine grained control when using `break` and `continue` in nested loops. Once labelled code can conditionally `break` or `continue` to the labelled loop by specifying the label after the keywords i.e. `break label` or `continue label`.

> [!example]
> Loop label syntax:
> ```javascript
> labelNameOuter: for(;;){
> 	// outer loop code body
> 	labelNameInner: for(;;){
> 		if(condition) break labelNameOuter 
> 	}
> }
>```

# [Loop The Loop](https://javascript.info/while-for)
---
Loops provide a mechanism to execute a certain code block for a specified number of iterations. JavaScript provides many loop structures such as `while`, `do..while` and the `for` loops.

All loops consisted of a condition, used to determine if the next iteration of the loop should be run, a [sentinel](https://en.wikipedia.org/wiki/Sentinel_value) which uses its presence as a condition for termination and a body which contains the code to be executed repeatedly. 

## Syntax
---

In a `while loop` and a `do..while` loop the sentinel is typically specified outside of the loop construct.

> [!tip]
> JavaScript allows the omission of `{}` around code blocks if they exist on a single line

```javascript
let sentinel = value;
while(conditionInvoldingSentinel){
	// code block is executed repeatedly while condition evaluates to true
	expressionToModifySentinel;
}
```

```javascript
let sentinel = value;
// The do..while loop executes the code block first before checking the condition
do {
	// code block is executed repeatedly while condition evaluates to true
	expressionToModifySentinel;
} while(condition);
```

In a for loop the sentinel is typically initialized within the `begin` expression` and modified in the `step` expression.

```javascript
for(begin; condition; step){
	// code block is executed repeatedly while condition evaluates to true
}

// the begin and step expressions can be skipped if the sentinel is initialized and modified outside the loop
for(; condition;){

}

// the condition expression can be omitted to create an everlsting loop 
for(begin;; step){

}
```

## `break` and `continue`
---
The `break` keyword can be used to exist a loop early. The `continue` keyword is used to skip the rest of the code body and continue from the next iteration. `break` and `continue` are typically combined with [[#[Conditional Branching](https //javascript.info/ifelse)|conditional branching]] in order to exit from a loop or skip an iteration conditionally.

> [!info]
>  `break` can be used inside any code block while `continue` can only be used inside loops

> [!warning]
> `break` and `continue` can not be used inside of the `?` operator.

Loops can be labeled to allow for more fine grained control when using `break` and `continue` in nested loops. Once labelled code can conditionally `break` or `continue` to the labelled loop by specifying the label after the keywords i.e. `break label` or `continue label`.

> [!example]
> Loop label syntax:
> ```javascript
> labelNameOuter: for(;;){
> 	// outer loop code body
> 	labelNameInner: for(;;){
> 		if(condition) break labelNameOuter 
> 	}
> }
>```

# [Loop The Loop](https://javascript.info/while-for)
---
Loops provide a mechanism to execute a certain code block for a specified number of iterations. JavaScript provides many loop structures such as `while`, `do..while` and the `for` loops.

All loops consisted of a condition, used to determine if the next iteration of the loop should be run, a [sentinel](https://en.wikipedia.org/wiki/Sentinel_value) which uses its presence as a condition for termination and a body which contains the code to be executed repeatedly. 

## Syntax
---

In a `while loop` and a `do..while` loop the sentinel is typically specified outside of the loop construct.

> [!tip]
> JavaScript allows the omission of `{}` around code blocks if they exist on a single line

```javascript
let sentinel = value;
while(conditionInvoldingSentinel){
	// code block is executed repeatedly while condition evaluates to true
	expressionToModifySentinel;
}
```

```javascript
let sentinel = value;
// The do..while loop executes the code block first before checking the condition
do {
	// code block is executed repeatedly while condition evaluates to true
	expressionToModifySentinel;
} while(condition);
```

In a for loop the sentinel is typically initialized within the `begin` expression` and modified in the `step` expression.

```javascript
for(begin; condition; step){
	// code block is executed repeatedly while condition evaluates to true
}

// the begin and step expressions can be skipped if the sentinel is initialized and modified outside the loop
for(; condition;){

}

// the condition expression can be omitted to create an everlsting loop 
for(begin;; step){

}
```

## `break` and `continue`
---
The `break` keyword can be used to exist a loop early. The `continue` keyword is used to skip the rest of the code body and continue from the next iteration. `break` and `continue` are typically combined with [[#[Conditional Branching](https //javascript.info/ifelse)|conditional branching]] in order to exit from a loop or skip an iteration conditionally.

> [!info]
>  `break` can be used inside any code block while `continue` can only be used inside loops

> [!warning]
> `break` and `continue` can not be used inside of the `?` operator.

Loops can be labeled to allow for more fine grained control when using `break` and `continue` in nested loops. Once labelled code can conditionally `break` or `continue` to the labelled loop by specifying the label after the keywords i.e. `break label` or `continue label`.

> [!example]
> Loop label syntax:
> ```javascript
> labelNameOuter: for(;;){
> 	// outer loop code body
> 	labelNameInner: for(;;){
> 		if(condition) break labelNameOuter 
> 	}
> }
>```

