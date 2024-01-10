---
tags:
  - draft
  - backend
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/array-methods
---

`delete arr[idx]` can be used to delete an item from an array
this makes sense as arrays are objects with `idx: value` properties
however `delete` does not modify the length of the array or internally rearrange elements to preserve contiguity.
using `delete` leaves an empty slot at `idx`. 

info negative indexing is supported for all array methods
`callback = (item, index, array) => {...}`

`arr.splice(start[, delCount, elem1, ..., elemN])`
modifies the array starting from index `start`, deletes `delCount` elements, then inserts `elem1` to `elemN` in the order specified and returns the array of removed elements(empty if no removed elements)
`start` can be negative: specifying the index from the back of the array
`splice` can be used for 
deleting 
inserting
deleting and inserting simultaneously 

`arr.slice([start], [end])`
return a copy of the array from `start` to `end`
if `end` is not specified or `> arr.length` it becomes `arr.length`
if both arguments are omitted then a copy of the whole array is returned.

`arr.concat(arg0, arg1, ..., argN)`
returns a new array containing items from `arr`, then `arg0`, …, `argN`
if the argument is a value it is placed in the new array as is
if the argument is an array or array-like object with the [`Symbol.isConcatSpreadable`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/isConcatSpreadable) property then its values are copied over 


`arr.forEach(callback)` invokes  `callback` for each element of the array. Passing in the item, its index and the array itself as arguments 


`arr.indexOf(item[, start])` searches from `start ?? 0` and returns the index of `item` in `arr` or `-1` if `404`.
`indexOf` uses `===`. therefore `indexOf(NaN)` always returns `false`

`arr.includes(item[, start])` is identical in function but returns a `boolean` value indicating whether or not `item` is in `arr`. 
`includes` handles `NaN` values correctly 

`arr.find(callback)` invokes `callback` on each element of `arr` and returns the first element for which the callback returns `true` else it returns `undefined`

`arr.findIndex` is identical in function but returns the index  of the passing element or `-1`
`arr.findLastIndex` is identical to `arr.findIndex` but operates back to front.

`arr.filter(callback)` returns a new array containing only the items for which `callback` return true when called on.

`arr.map(callback)` invokes `callback` on each element of the array and passes the return value into a new array

`arr.sort(sortingFunction)` sorts the array in place using `sortingFunction` and also returns the sorted array.
the default `sortingFunction` converts all elements to strings prior to comparison
`sortingFunction(a, b)` takes two values and returns:
- a positive number if `a > b`
- a negative number if `a < b`
- `0` if `a === b`

`arr.sort((a, b) => a - b)` to sort numbers
`arr.sort((a, b) => a.localeCompare(b))` to sort strings

`arr.reverse()` reverses `arr` in place and returns `arr`.

`str.split(delim[, maxLength])` splits `str` into an array separating on every occurrence of `delim` stopping if the length of the returned array `=== maxLength`
`arr.join(glue)` joins `arr` into a string placing `glue` in between each element
note `.split()` and `.split("")` split on individual letters

`arr.reduce((accum, elem, idx, array) => {...}[, initial])`

#incomplete 
