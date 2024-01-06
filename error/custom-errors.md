---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/custom-errors
---
The built-in error classes can be extended to provide scenario specific, descriptive errors.

> [!note]
> Reassign the `name` of a custom error class to `this.constructor.name` immediately after the invocation of `super(...)`

> [!tip]
> Prefer `instanceof`  as opposed to `error.name === "TargerError"` as this also catches derived classes of the target error

> [!example]
> A `ValidationError` class that extends `Error` and represents a failure during the validation of a `JSON` object due missing properties, wrong types in certain properties e.t.c.  
> 
> A validation function runs validation logic and runs:
> - `throw ValidationError` 
> - `throw ValidationErrorSubClass`.  
> 
> The calling can then watch for this errors using a `try...catch` block.

***wrapping exceptions*** 
actually bullshit, just watch for the `BaseClass`
