---
source: https://javascript.info/async-iterators-generators
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
---
Asynchronous iteration allows for looping over values that come asynchronously.

Implementing the `Symbol.asyncIterator` method makes an object an *asynchronous iterable*.
`Symbol.asyncIterator` is expected to return an *asynchronous iterator*  
*asynchronous iterator* is an object that implements the `next: () => Promise<{ done: boolean, value: any}>` method

the `for await..of` loop construct allows for iteration over asynchronous iterables 

```javascript
async function f(){
  for await(const value of asyncIterable){...}
}
```

When an async iterable is used in a `for..await` loop the following procedure begins:
1. `Symbol.asyncIterator`  is invoked and obtains the async iterator
2. `for..await` calls the `next` method which returns a promise
3. the loop execution is then asynchronously paused until the promise resolves
4. once this happens the `value` is taken from the resolved object and passed to the calling code
5. the body of the loop executes 
6. steps 2 to 5 repeat until the `done: false` on the resolved object

Using asynchronous iteration to make a clock.

```javascript
let clock = {
  from: 0,
  to: 10,

  [Symbol.asyncIterator]() {
    return {
      current: this.from,
      last: this.to,

      next() {
        return new Promise(resolve => setTimeout(resolve, 1000, { 
          done: this.last <= this.current,
          value: this.current++
        }));
      }
    };
  }
};

(async () => {

  for await (let value of clock) { 
    console.log(value); // 0, 1, 2, ..., 9
  }

})()

```

note:
  `for..await` has to be used inside an `async` function

`async` can be used on the `next` method of the async iterator to take advantage of `async`/`await`

```javascript
let clock = {
  from: 0,
  to: 10,

  [Symbol.asyncIterator]() {
    return {
      current: this.from,
      last: this.to,

      async next() {
        await new Promise(resolve => setTimeout(resolve, 1000));

        return {
          done: this.last <= this.current,
          value: this.current++
        }
      }
    };
  }
};

(async () => {

  for await (let value of clock) { 
    console.log(value); // 0, 1, 2, ..., 9
  }

})()
```

