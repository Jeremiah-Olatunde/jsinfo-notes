---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
  - draft
source: https://javascript.info/optional-chaining
---
Optional chaining provides a neat, concise and safe way to access nested properties of an object. `obj?.objProp?.objProp?.prop`.
if the value before the `?.` is `undefined` or `null` the evaluation of the expression is short circuited `undefined` is returned.
note that only the value before `?.` is optional
the variable before `?.` must be declared.
`?.` is not an operator but a special syntax construct
this mean that it can be used with function calls `?.()`
or with the [[object#Accessing properties|square bracket syntax]] `?.[]`
warn `?.` can be used for safe deleting and reading but not writing.