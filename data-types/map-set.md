---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/map-set
---
Map is a collection of keyed data items.
keys may be of any type at all, unlike object (strings and symbols).

```javascript
// creates a new empty map
new Map([iterableKeyValue]); 

// sets key => value returns map
// if key in map updates values
map.set(key, value); 

map.get(key); // returns value of key of undefined if no key
map.has(key); // boolean indicating key presence 
map.delete(key); // removes key and its value
map.clear(); // empties map
map.size; // number of elements in map
```

`new Map(iterableKeyValue)`
`Map` constructor takes any iterable with key value pairs
example: Array
	`[]` throws an error
	`[[]]` adds  `undefined => undefined`
	`[[], [], []]` add a single  `undefined => undefined` (`undefined === undefined`)
	`[[x], [y]]` map with `x => undefined` `y => undefined`
example: Object
	Use [`Object.entries`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) to get the iterable `[key, value]`
	`new Map(Object.entries(obj))`

note:
	`Map`s are objects
	Object syntax can be used to add properties to maps i.e. `map[key]` or `map.key`
	reimposes same limitations as those on object keys (strings or symbols)
	actually it just uses map as an object
	`get` and `set` do not work with the object properties of map at all (example)

info
	map uses the [SameValueZero](https://tc39.es/ecma262/#sec-samevaluezero) algorithm to compare its key
	identical to `===` but considers `NaN` equal to other `NaN` values
	`NaN` can be used as a key

iteration
map can be looped over with
```javascript
map.keys(); // returns an iterable/iterator for the keys
map.values(); // returns an iterable/iterator over the values
map.entries(); // returns an iterable/iterator over entries [key, value]
// they return objects that are both iterable and iterators

// note map itself is iterable
// by default it iterates over [key, value] i.e. map.entries
for(const [key, value] of map);
// is equivalent to
for(const [key, value] of map.entries);

map.forEach((value, key, map) => { // note value before key in argument order
	// code 
});
```

note: Ordered
	map stores items in insertion order 
	`keys`, `values`, `entries` work with insertion order


[`Object.fromEntries(iterableKeyValue)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/fromEntries)
use to create an object from `map.entries` or just `map`

Set
A collection of unique values
```javascript
new Set([iterableValue]); // create a new Set
set.add(value); // adds value if value ∉ set returns set
set.delete(value); // remove value and return true. if no value return false
set.has(value); // true if value ∈ set
set.clear(); // empties set
set.size; // no of elements
```

note:
	An array with `arr.find` could be used with as opposed to set 
	`O(n)` performance while `Set` is `O(1)`

iteration
```javascript
set.keys(); // iterable over values
set.values(); // iterable over values
set.entries(); // [value, value] iterable
// compatibility with map

for(const value of set); // iterates over values

set.forEach((value, value, set) => { // value passed twice for compatibilty with map

})
```