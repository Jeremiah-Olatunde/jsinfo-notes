---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/weakmap-weakset
---
JavaScript keeps reachable objects in memory
elements of a any data structure are reachable 
example:
	object in an array
	objects in a map as a value or key

`WeakMap`
keys of `WeakMap` must be objects 
when the only reference to an object is the key in a `WeakMap` it is garbage collected.
i.e. `WeakMap` references to objects are not considered by the garbage collector

`WeakMap` provides only `set`, `get`, `has`, `delete` methods which are identical to `Map`


`WeakMap` do not support iteration i.e. `keys`, `values`, `entries` methods.
Objects can loose their strong references at anytime
the garbage collector arbitrarily decides when memory clean up of these objects happen
therefore the current element count of a `WeakMap` is unknown
an object that has already lost its reference might be iterated over 
the idea is to only allow access to the map via external references to the object keys
this ensures that there is indeed always a strong reference because obviously
remember the keys are only objects 
if we were iterating over the map we would have access to the `WeakMap` elements via only the weak references and we would not know whether or not strong references exist
and we could be iterating over objects that would soon be cleared from memory

use cases for `WeakMap`
store data related to an object that should be deleted if the object is deleted (garbage collected) as well.
using a `Map` would be that the map would have to be manually cleared
use case visit count
caching


`WeakSet`
Only allows object values
does not support any of the iteration methods
used for additional storage of *yes/no* facts as opposed to arbitrary data
membership in a `WeakSet` can confer information about the object
A `WeakSet` of users who have visited a site 
