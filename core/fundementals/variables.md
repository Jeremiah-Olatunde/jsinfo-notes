---
tags:
  - javascript
  - jsinfo
  - frontend
  - backend
source: https://javascript.info/variables
banner: "![[RDT_20211230_2342055806120662248886414.jpg]]"
banner_y: 0.53429
---
# Variables
---
Variables are named storage locations for data. In JavaScript variables are declared using the `let` keyword. Variables can only be declared once but can be reassigned to a different value.

```javascript
let varName = "hello world";
varName = "goodbye world"
```

Variable names in JavaScript are case sensitive and can contain ***only*** numbers, letters and the symbols `$` and `_`. Variable names can not be any of the JavaScript keywords such as `for`, `let`, `undefined` etc..

> [!warning] The first character of a variable name can not be a digit.

Constants are declared using the `const` keyword. Once declared the value of constant can not be changed. 

> [!info]
> Constants that are known prior to execution time are typically named using all uppercase characters while constants computed at run time are named normally.

