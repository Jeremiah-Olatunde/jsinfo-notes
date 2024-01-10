---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/iterable
---
Iterable objects are a generalization of arrays allowing any object to be made useable in a `for...of` loop.

note: iterable vs iterators
	an iterable object is one that implements `[Symbol.iterator]`
	an iterator is an object with a `next: () => { done: boolean, value: any}` method. 

`Symbol.iterator`
The `Symbol.iterator` system [[symbol]] enables the creation of iterable objects.
The `Symbol.iterator` is used to create a method on the target object 
`Symbol.iterator` method returns an iterator (i.e. an object with a `next`) method.
Then `next` method returns an object of signature `{ done: boolean, value: any }` each time it is called.

When the iterable object is used in a `for..of` loop the following procedure begins:
1. `for..of` loop calls the `[Symbol.iterator]` method and obtains the iterator
2. `for..of` calls the `next` method passing the value of the `value` property to the calling code
3. if the `done` property is `false` `for..of` moves on to the next iteration calling `next` again
4. if `done` is `true` `for..of` exits

example: implementation of a range object
note: separation of concerns. 
	`Symbol.iterator` returns a new object not the original
	this allows for multiple calls to `Symbol.iterator` in multiple simultaneous `for..of` loops
	example

note:  infinite iterators
	next().done` is never `true`
	manual `break`

Strings are iterable.
Iterating over them with `for..of` returns each charater
note: Surrogate pairs
question: what are surrogate pairs

explicitly calling an iterator 


iterables: defined above.
array-likes are objects with numerical indexes and a `length` property.

some iterables may be array-likes and vice-versa.
Some iterables are not array-likes and vice-versa e.g. range

`Array.from(obj[, mapFn, thisArg])`
takes an iterable or array-like and make an actual array from it
takes an optional mapping function to transform the values prior
optionally set the this argument

example: `Array.from(string)`
	relies on the iterable nature of the string
	hence works on surrogate pairs 
	equivalent to using a `for..of`
	build a surrogate aware slice