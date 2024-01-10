---
tags:
  - backend
  - javascript
  - frontend
  - jsinfo
source: https://javascript.info/while-for
banner: "![[ying&yeng.jpg]]"
---
# Loops: `while` and `for`
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

