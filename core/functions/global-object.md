---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/global-object
banner: "![[titanfall-2-wallpaper-1920x1080-explosion-concept-art-hd-3270.jpg]]"
---
The global object provides variables and functions that are available every in every scope as properties and methods
by default these include language built-ins
`globalThis` is the language standard name for the global object
In the browser it is also `Window`, in NodeJS it is also `global`

note the difference between the global object `window` and the `Window` function constructor

in the browser all variables and functions declared using `var`, and functions declared with function declaration syntax in the global scope automatically become properties of the global object
warning:
	`var` ignores block scopes [[var]]
	function declarations do not

this does not occur in JavaScript Modules

warning:
	global variables are heavily discouraged 
	if using them is a must explicitly assign them as properties of the global object

use for polyfills