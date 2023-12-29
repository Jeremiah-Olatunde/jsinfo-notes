---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/structure
banner: "![[RDT_20211230_2346222555412364986717688.jpg]]"
banner_y: 0.58571
---
# Code structure
---
Statements are are syntax structures and command that perform actions. Each statement is placed and its own line and ends with the `;`. JavaScript supports [automatic semicolon insertion](https://tc39.es/ecma262/#sec-automatic-semicolon-insertion).

> [!warning]
> Automatic semicolon insertion can *"fail"* in some cases e.g.
> ```javascript
> alert("hello world")
> [1, 2].foreach(alert);
> 
> function returnsUndefined(){
> 	 return
> 		"this value is ignored";
> }
> ```

Single line comments in JavaScript are specified with the `//` at the start of the line. Multiline comments are surrounded with `/*` and `*/`.
