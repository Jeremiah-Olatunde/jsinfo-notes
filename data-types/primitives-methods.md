---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/primitives-methods
---
# Methods of primitives
---

For ease and convenience JavaScript allows primitive values to be operated on by calling methods on them. 

In order to achieve this JavaScript creates special wrapper objects around the primitives that provide methods and properties required to operate on the wrapped primitive value. 

The wrapper objects are created only when a method or property of a primitive is accessed and destroyed immediately after the operation. 

This method allows for primitives to be operator on like objects for convenience but still remain *"lightweight"* and fast.

Every primitive has its own object wrapper (`String`, `Number`, `Boolean`, `Symbol` and `BigInt`) each provide a tailored set of methods and properties.

note that JavaScript engines highly optimize the process described above: in some cases skipping it all together. 

The object wrapper functions `String`, `Number`, `Boolean` are typically used to convert between types.

warning
The wrapper objects can manually be created using the `new` keyword to make instances of the corresponding classes however this is highly **unrecommended**. 
The engine performance optimizations are diminished.
Weird things happen

`typeof 0 === number` but `typeof new Number(0) === "object"`
`Boolean(0) === false` but `Boolean(new Number(0)) === true` object are always truthy
