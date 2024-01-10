---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/comparison
banner: "![[RDT_20211230_2341538258584312757593932.jpg]]"
banner_y: 0.77999
---
# Comparisons
---
Comparison operators compare two values of similar or different types and return a `boolean` indicating the result of the comparison. For `number` to `number` comparisons the results are as expected. For other types JavaScript performs type conversions according to a specific set of rules.

## String Comparisons
---
Strings are compared by Unicode order.  The algorithm is as follows:

1. Compare the first character of both strings.
2. If the first character from the first string is greater (or less) than the other string’s, then the first string is greater (or less) than the second. We’re done.
3. Otherwise, if both strings’ first characters are the same, compare the second characters the same way.
4. Repeat until the end of either string.
5. If both strings end at the same length, then they are equal. Otherwise, the longer string is greater

## Comparing Differing Types
---
When two values of different types are compared, JavaScript converts both values to `number` type using the rules described in [[#Numeric Conversions]].

> [!warning] JavaScript Weirdness
> Two values can be `==` to each other while their `boolean` forms are `!=`
> ```javascript
> const a = 0;   // Boolean(0) == false
> const b = "0"; // Boolean("0") == true
> 
> a == b // true 
> Boolean(a) == Boolean(b) // false
> ```


## Strict Equality (`===`, `!==`)
---
The `==` and `!=` checks performs numeric type conversion when comparing values of different types. This leaves it unable to distinguish between `0` and `false` or `''` and `false` as the all convert to `0` prior to the equality check. The strict equality operators `===` and `!==` do not perform type conversion on their operands.

## Comparisons between `null` and `undefined`
---
When `null` is compared to `undefined` using strict equality checks the result is false as they are each of differing types. For non-strict checks `null` and `undefined` are only equal to each other *(sweet couple)* and themselves, not any other value. For other math comparisons `null` and `undefined` are [[#Numeric Conversions|converted to numbers]].  

> [!warning] JavaScript Weirdness
> `null` vs `0`
> ```javascript
> null > 0 // false (null is converted to 0)
> null == 0 // false (null is only == to null and undefined)
> null >= 0 // true (null is converted to 0)
> // wtf javascript
> ```
> incomparable undefined
> ```javascript
>undefined > 0 // false (undefined converted to NaN)
>undefined < 0 // false (NaN returns falls for all comparisons)
>undefined == 0 // false (undefined is only == to undefined and null)
>// like...why?
>```


## Tasks
---
#coding-problems/javascript/jsinfo 

What will be the result for these comparisons

```javascript
5 > 4                 // true
"apple" > "pineapple" // false
"2" > "12"            // true
undefined == null     // true 
undefined === null    // false
null == "\n0\n"       // false
null === +"\n0\n"     // false
```
