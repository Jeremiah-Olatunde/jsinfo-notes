---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/operators
banner: "![[RDT_20211230_2340276559390054174659129.jpg]]"
---
# Basic Operators, maths
---
An *operand* refers to what operators are applied to. In the expression `5 + 2` the `+` operator has two operands `5` and `2`.  An operator is *unary* if it has a single operand i.e. Boolean inversion (`!`), negation (`-`).  An operator is *binary* is it has two operands i.e. addition `+`,  subtraction (`-`).

## Math Operators
---

operation | operator
--- | --- 
addition | `+` 
subtraction | `-`
division | `/`
multiplication | `*`
exponentiation | `**`
modulo | `%`

## String Concatenation
---
The binary `+` operator is overloaded. When applied to `string` it concatenates them. If any of the operands is of type `string` the other is converted to `string` type as well. The `+` is left associative meaning it is computed left to right. 

> [!example]
> Basic string concatenation
> ```javascript
> "hello" + " " + "world"
> ```
> Type conversion of one of the operands
> ```javascript
> "1" + 2 // "12"
> ```
> Type conversion combined with left associativity
> ```javascript
> 2 + 2 + "1" // "41"
> "2" + 2 + 1 // "221"
> ```

> [!note]
> Other mathematical operators are not overloaded in the same way and hence convert their operands to `number` type.
> ```javascript
> "6" / "2" // 3
> 6 - "2" // 4
> ```

## Numeric Conversion
---
The unary `+` operator converts no `number` data types to numbers. It is the short hand for `Number(...)`.

```javascript
+"2" + +"3" == 5 // true
"2" + "3" == 23 // true
```

## Operator Precedence
---
Every operator in JavaScript has a precedence which determines order in which they are applied during the evaluation of expressions with multiple operators. Operators with higher precedence are always applied first. If the precedence of two operators are the same they are executed left to right.

The assignment operator has an extremely low precedence. The operator also returns the value that was assigned to the variable *(This only occurs during reassignment)*. `x = value` assigns `value` to `x` and returns `value`. This allows assignments to be used as part of a larger expression and for variable assignments to be chained.

```javascript
let a, b, c;
a = b = c = 2 + 2; // a = 4, b = , c = 4
a = 3 - (b = c + 1); // b = 5, a = -2
```

## Modify-in-place
---
Modify in place operators apply an operator to a variable and store the result in said variable which leads to more concise code. Modify in place operators exist for all arithmetic and bitwise operators and have the same precedence as a the regular assignment operator. The syntax is the operator followed by the assignment operator e.g. `+=`, `-=`, `*=`, `**=` 

## Increment/Decrement Operators
---
The increment `++` and decrement `--` operators increase and decrease the value of a **variable** by one respectively. They can be applied to a variable in either of two ways: *prefix* (`++var`) or *postfix* (`var++`) form. Prefix form increments or decrements the variable by one and returns the **new value**. The postfix form increments or decrements the variable by one and returns the **old value**.

## Bitwise Operators
___
Bitwise operators treat their arguments as 32-bit binary integers.

| operator             | symbol |
| -------------------- | ------ |
| AND                  | `&`    |
| OR                   | `|`    |
| XOR                  | `^`    |
| NOT                  | `!`    |
| LEFT SHIFT           | `<<`   |
| RIGHT SHIFT          | `>>`   |
| ZERO-FILL LEFT SHIFT | `<<<`  |
| ZERO-FILL RIGHT SHIFT                     | `>>>`  |

## Comma Operator
---
The comma operator allows for the evaluation of serval expressions (each separated by a `,`) and returns only the result of the last expression. This allows for one-liners 

> [!example]
> Using the comma operator to assign multiple variables in a for loop
> ```javascript
> for(a = 1, b = 3, c = a * c; a < 10; a++){
> 	// code
> }
> ```

> [!note] Precedence
> The comma operator has a lower precedence than the assignment operator
> ```javascript
> const a = (1 + 2, 3 + 4); // a = 7
> const b = 1 + 2, 3 + 4;   // throws and error
>```

## Tasks 
---
#coding-problems/javascript/jsinfo

What are the results of these expressions

```javascript
              // Answers
"" + 1 + 0    // "10"
"" - 1 + 0    // -1
true + false  // 1
6 / "3"       // 2
"2" * "3"     // 6
4 + 5 + "px"  // "9px"
"$" + 4 + 5   // "$45"
"4" - 2       // 2
"4px" - 2     // NaN
"  -9  " + 5  // "  -9  5"
"  -9  " - 5  // -14
null + 1      // 1
undefined + 1 // NaN
" \t \n" - 2  // -2
```
