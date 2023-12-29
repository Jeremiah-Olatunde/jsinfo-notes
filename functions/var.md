---
tags:
  - backend
  - draft
  - frontend
  - jsinfo
  - javascript
source: https://javascript.info/var
banner: "![[1581769.jpg]]"
banner_y: 0.22002
---
variables can be declared using the depreciated `var` keyword
there are important differences between `var` and `let`/`const`

`var` ignores block scopes.
`var` variables can only be function-scoped or global-scoped
examples: 
	`if`, `for`, `while`, `{}`(code blocks)

note: 
	if `var` is used in a function then it is a function-level variables
	ignores block scopes

`var` tolerates redeclarations

`var` variables are hoisted to the top of their scopes like functions
there are not `uninitialized` like `let`/`const` declared variables 
however unlike functions their values are not assigned yet and hence is `undefined`
that is to say `var` declarations are hoisted but now their assignments

IIFE
immediately invoked function expressions
used to emulate block-scoping when using `var` declarations
creating functions expressions  that are immediately invoked as opposed to assigned to a variable
wrap in parentheses to indicate that it is an expression then call.
note other syntaxes