---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/array
---
`Array` type is used to store ordered collections
Arrays are objects with `key: value` properties. 
The `key` in array properties are [[object#Property Ordering|integer properties]] representing the index (or position) of the `value` in the ordering.
An array can store values of multiple types similtaneously.

new empty array are created with the array literal syntax `const arr = []` or the array constructor syntax `new Array()`
elements can be inserted on creation by placing them within the `[]` or as arguments to `new Array()` constructor and separating them by commas
tip arrays of arrays (multidimensional arrays) can be used to store matrices

warning `new Array()` called with a single `number` argument creates an empty array with `number` empty slots.

the [[object#Square bracket syntax|square bracket syntax]] is used to access array items using the index of an element as the key
`arr[idx]`. 
items can be replaced or new items added to new indexes using the syntax. as with properties on object.
as with properties on object accessing an index that does not exist returns undefined.

note 
- what happens when a item is added at an index that is not the last index + 1.
- what happens when a non-[[object#Property Ordering|integer property]]  key is used and it's effect on `length`
- 
the `length` property stores the length of the array.
tip trailing comma

`arr.at(idx)` method can be used to access elements in an array
negative indexing is allowed unlike with `[]` which return undefined.

`arr.pop()` removes the last element and returns it or `undefined`
`arr.push(elem0, ..., elemN)`appends all the arguments to the array in the specified order and returns the new `length` of the array

`arr.shift()` removes the first element of an array and returns it or `undefined`
`arr.unshift(elem0, ..., elemN)` prepends all arguments in specified order to the array and returns new length

note calling `.unshift` or `.push` repeatedly does not equal passing the arguments in all at once

internals
arrays are objects
there are passed by reference
arbitrary properties can be added to them
JavaScript engines work with them differently from objects to improve performance. 
Array elements are attempted to be stored in contiguous ordering in memory
however using arrays as objects breaks JavaScript engine opimitisations
- adding non-integer properties
- making holes in the array
- filling the array in reverse order

note the algorithmic complexities
`push` and `pop` are `O(1)`
`shift` and `unshift` are `O(n)`

looping
[[while-for#Loops `while` and `for`|`for` loops]] are traditionally used to iterate through the elements of an array
the sentinel is initialized at the desired starting index (typically `0`)  and incremented (usually by `1`) until the desired stopping index is reached (normally `arr.length`).

`for..of` loops can be used to iterate through the elements of an array as well. Unlike with the traditional `for` loop the index is not available via the sentinel. Although technically you could keep an external sentinel similar to a while loop

`for..in` loops can be used to iterate through the ***properties*** of an array.
note that this iterates through the regular elements (those stored at [[object#Property Ordering|integer properties]]) and the non-[[object#Property Ordering|integer properties]] as well such as `length`.

warning
using `for..in` loops are much slower than `for` or `for..of` loops.
`for..in` loops clash with *array-like* object (Iterables).

the `length` property represents not the number of elements in an array but the greatest integer property (numeric index) + 1.
idea what happens with `arr[Infinity] = value` 
length can be manually altered. increasing it fills the array with empty slots
decreasing it truncates the array
`arr.length = 0` clears the array
question what does this truncation do to memory management and garbage collection

toString
arrays implement a custom `toString` method that returns a comma-separated list of the elements.
if there are no elements an `""` is returned
arrays to not implement `valueOf` or `Symbol.toPrimitive` and hence based on the [[object-toprimitive|object to primitive conversion rules]] they are always converted to strings when type coercion occurs. 

comparisons
Arrays are compared just like objects
They are [[object-copy#Comparison by reference|compared by reference]] to other arrays and object
`==` performs [[object-toprimitive|type coercion]] when comparing to primitives
`===` strict equality does not

warning
the sole implementation of `toString` on array and the comparison rules for objects lead to some weird situations. during `==` type conversions arrays always get converted to strings first


read later
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) article on arrays. covers empty slots and how array methods interact with them