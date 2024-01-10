---
tags:
  - backend
  - frontend
  - draft
  - javascript
  - jsinfo
source: https://javascript.info/generators
---
generator functions provide a more concise way to implement iterators.

syntax

```javascript
function* generator(){
  yield value0;
  yield value1;
  yield value2;
  return value3; // return undefined by default
}
```

calling a generator function returns a generator object which is an iterator
i.e. an object with a `next: () => { done: boolean, value: any}` method.
the generator object controls the execution of the generator function

when `next` is called the generator function executes until it hits the first `yield someValue` and then pauses.  `next` returns then returns `{ done: false, value: someValue}`

calling `next` again resumes the execution of the generator function until the next `yield someOtherValue`. again the execution is paused and `{ done: false, value: someOtherValue }` is returned.

this cycle continues until the `return` statement.
once `return` is reached `next` returns `{ done: false, value: returnedValue }`

note:
  functions in JavaScript have an implicit `return undefined` if `return` is omitted 
  hence `someValue` becomes `undefined`


the object returned when the generator function is called is an iterator
this means it can be used in `for`/`of` loops and with the spread syntax

```javascript
function* F(){
  yield 0;
  yield 1;
  yield 2;
  return 20;
}


for(const value of F()) console.log(value);

// 1
// 2
// 3

console.log([...F()]); // [1, 2, 3]
```

note:
  `for`/`of` loop exists when `done: true` without executing the loop body
  hence the value ***returned*** from the generator function is ignored 

***using generators for iterables***

recall the procedure followed when an iterable is used in a for of loop

the `Symbol.iterator` method can be turned into a generator function by prepending a `*`  
this means it returns a generator object is an iterator (which is what a for of loop expects).  

`yield` can then be used to *"return"* values from the iterator

```javascript
const range = {
  start: 0,
  end : 10,
  *[Symbol.iterator](){
    // using i instead of mutating the object state allows
    // the iterable to be used multiple times
    for(let i = 0; i < this.end; i++){
      yield i;
    }
  }
}
console.log([...range]); // [0, ..., 9]
console.log([...range]); // [0, ..., 9]
console.log([...range]); // [0, ..., 9]
```

tip:
infinite generators are possible.

***generator compositions***

generators can delegate the generation and yielding of values to other generators using the yield from syntax `yield* generatorObject` 

it automatically iterates over `generatorObject` and then yields the values it generates.

```javascript
function* RandomGenerator(count, min, max){
  while(count-- > 0)
    yield Math.floor(Math.random() * (max - min) + min)
}

function* SequenceGenerator(start, end, increment){
  while(Math.min(start, end) < Math.max(start, end)){
    yield start;
    start += increment;
  }
}


function* ComposedGenerator(){
  yield* RandomGenerator(5, 0, 100);
  yield* SequenceGenerator(0, 10, 2);
}


console.log([...ComposedGenerator()]) // [ 13, 67, 76, 89, 63, 0, 2, 4, 6, 8 ]
```

`generatorObject.next(value)`

`yield` is an expression ergo it returns a value  
`next` takes an argument, this argument becomes the result of the `yield` expression.
the argument of the first `next` call is discarded (it starts the generator execution)

```javascript
function* F(){
  console.log(yield 2); // hello
}

const f = F();

console.log(f.next("ignored")); // 2
console.log(f.next("hello")); // undefined
```

`generatorObject.throw(value)`
throw an error inside the generator object.
the generator function can use `try`/`catch` to handle the error
if the error is not handled it falls out of the generator


`generatorObject.return(value)`
forces the generator to finish its execution prematurely
returns `{ done: true, value: value }`

```javascript
function* gen() {
  yield 1;
  yield 2;
  yield 3;
}

const g = gen();

g.next(); // { value: 1, done: false }
g.return("foo"); // { value: "foo", done: true }
g.next(); // { value: undefined, done: true }
```

