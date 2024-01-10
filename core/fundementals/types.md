---
tags:
  - javascript
  - jsinfo
  - frontend
  - backend
source: https://javascript.info/types
banner: "![[RDT_20211231_0541501030891549299143210.jpg]]"
banner_y: 0.5
---
# Data types
---
All values in JavaScript are of a certain type. There are seven primitive data types in JavaScript.: `Boolean`, `number`, `bigint`, `string`, `undefined`, `null`, `symbol`. JavaScript is dynamically typed meaning values of different types can be assigned to variables arbitrarily.

## `number`
---
The `number` type represents both integer and floating point numbers. `Infinity`, `-Infinity` and `NaN` are known as *special numeric values*. `Infinity` is the result of a division by zero and is always greater than an number during comparisons.  `NaN` represents a computational error and is the result of incorrect or undefined mathematical operations. 

> [!info] 
> - `NaN` is sticky meaning a `NaN` anywhere in a series of mathematical operations propagates to the whole result.
> - `NaN` returns `false` for all comparisons; even to itself.

> [!warning] JavaScript Weirdness
> `NaN` raised to the power of `0` returns `1`.
> ```javascript
> NaN ** 0 == 1 // true
> ```

## `BigInt`
---
The `BigInt` type represents extremely large numbers outside of the range of what can be safely represented in JavaScript with the `number` type. A `BigInt` value is created by appending `n` at the end of a number type.

```javascript
const yourMomsWeight = 987654321987654321987654321987654321n
```

## `string` 
---
Strings in JavaScript are surrounded by single or double quotes. JavaScript lacks a `char` data type, only `string` therefore  `""` and `''` are equivalent. JavaScript support value embedding inside of strings with the backtick syntax ``const str = `hello ${expression}`;`` .

## `boolean`
---
The `boolean` data type represents `true` or `false` values. They are the result of comparison operators.

> [!example]
> Randomly return `true` or `false`
> ```javascript
> const bias = 0.5;
> const random = () => Math.random() < bias;
>```

## `null` and `undefined`
---
The `null` type represents a non-existing object or value that is unknown at that point in time. `undefined` represents a variable to which a value has not yet been assigned.

## `object` and `symbol`
---
The `object` type represents a value that stores a collection of data in `key: value` pairs. The `symbol` type is used to create unique identifiers for objects.

## The `typeof` Operator
---
The `typeof operand` operator return the type of the operand provided.

```javascript
typeof undefined // "undefined"

typeof 0 // "number"

typeof 10n // "bigint"

typeof true // "boolean"

typeof "foo" // "string"

typeof Symbol("id") // "symbol"

typeof Math // "object"  (1)

typeof null // "object"  (2)

typeof alert // "function"  (3)
```

> [!warning] JavaScript Weirdness
> - `typeof null` returns `"object"` which is officially recognized as a language error. `null` is a special value with its own separate type.
> - `typeof alert` returns `"function"` even though functions in JavaScript are themselves of the `object` type.
