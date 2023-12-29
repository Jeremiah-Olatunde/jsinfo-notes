---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
  - draft
source: https://javascript.info/object-toprimitive
---
When operators are applied to objects they care automatically converted to the primitives required by said operators and the operation carried out.
This mean the result of applying operators to objects will always be a primitive and never another object.
Furthermore when objects are passed into functions that require a certain primitive, such as `alert` or `Math.round` they are converted into the required primitive.

All objects are `true` in the `boolean` context.
Numeric conversions occur when mathematical operators or functions (`+`, `Math.floor`) are applied to objects.
String conversations occur when passing objects into functions like `alert`.

There are three variants of type conversion that occur in different situations. These variants are described by *hints*. These *hints* are `"string"`, `"number"` and `"default"`.
the `"string"` hint happens when an object is used in an operation that expects a string.
the `"number"` hint occurs when an object is used in a operation that expects a number such as with math operators, or the unary `+`.
and the `"default"` hint occurs when the exact type expected by the operator is ambiguous such as with the `+` operator which works on both `string` and `number` types or when an object is compared to a primitive using `==`. 
note that no ambiguity occurs using the `===` to compare an object to a primitive because `===` does not perform type conversion and as such using it to compare an object to primitive always returns `false`.
`<` `>` operators like the `+` work on both `string` and `number` types but use the `"number"` hint and not the `"default"` for historical reasons.
all built-in objects implement the `"default"` hint identically to the `"number"` hint

The conversion algorithm is as follows
1.  try `obj?.[Symbol.toPrimitive](hint)`
2. otherwise if the hint is `"string"`: 
	1. call `obj.toString` if not `undefined` and doesn't return a non-primitive.
	2. otherwise call `obj.valueOf`
3. otherwise if the hint is `"number"`: 
	1. call `obj.valueOf` if not `undefined` and doesn't return a non-primitive.
	2. otherwise call `obj.toString`

The `Symbol.toPrimitive` [[symbol]] is used to implement a custom object to primitive conversion method for an object. The conversion method is assigned as the value to the property for which `Symbol.toPrimitive`. The method is passed the hint and can use this to implement the custom conversion logic.
warning throws error what happens when the `Symbol.toPrimitive` method returns a value other than `string` or `number`

if the `Symbol.toPrimitive` method does not exist then JavaScript fall back on the `toString` and `valueOf` methods in the manner specified in the algorithm above. All objects implement default versions of these functions
- The default `toString` returns `[object Object]`
- The default `valueOf` returns the object itself (triggering the invocation of `toString`).

`toString` and `valueOf` can return any arbitrary value, primitive or not. `Symbol.toPrimitive` can return any arbitrary primitive but it must be a primitive. If a primitive type other than the hinted primitive type is returned it is [[type-conversions|converted]] to the hinted primitive type. 

