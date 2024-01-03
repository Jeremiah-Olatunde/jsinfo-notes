---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/instanceof
---
`objA instanceof objB.prototype` returns `true` if `objB` is in the prototype chain of `objA`

```javascript
class Animal {}
class Rabbit extends Animal {}
console.log(new Rabbit() instanceof Animal); // true

function Person(){}
function Athelete(){}
Object.setPrototypeOf(Athelete.prototype, Person.prototype);

console.log(new Athelete() instanceof Person); // true
```


custom implementation of `instanceof`

```javascript
function mock({ __proto__ }, objB){
  if(__proto__ === null) return false;
  if(__proto__ === objB.prototype) return true;
  return mock(__proto__, objB);
}

console.log(mock(new Rabbit(), Animal)); // true
console.log(mock(new Athelete(), Person)); // true
```


that static method `Symbol.hasIntance` can be used to specify custom checking logic 
the method is passed an object and should return `true`/`false`
`obj instanceof Class` runs `Class[Symbol.hasInstance](obj)` and returns the result
if `Symbol.hasIntance` then the prototype chain is searched.

```javascript
class Animal {
  static [Symbol.hasInstance](obj) {
    return obj.canEat;
  }
}

console.log({ canEat: true } instanceof Animal); // true
```

note
this can not be used on function constructors as `Symbol.hasInstance` is read only

warning:
`objA instanceof objB.prototype` checks for `objB.prototype` not `objB` itself


`Object.prototype.toString` can be called on any value and returns a `string` of the form `[object Type]` e.g. `[object Number]`, `[object String]`, `[object Undefined]`

the `Symbol.toStringTag` can be used to specify the string used in place of `Type`