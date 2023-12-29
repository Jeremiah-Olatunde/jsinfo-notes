---
banner: "![[ying&yeng.jpg]]"
banner_y: 0.33429
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/recursion
---
# Recursion and stack
---

power function in an iterative and recursive form

```javascript
const pow = (x, n) => n == 0 ? 1 : x * pow(x, n - 1);
```

```javascript
function pow(x, n){
	let res = 1;
	for(let i = 0; i < n; i++) res *= x;
	return res;
}
```

maximum number of recursive calls is typically 1000.
depends on the JavaScript engine and optimizations like tail call optimization

# The execution context and the stack
---
The execution context is an internal data structure that contains data about the currently executing function:
- the value of `this`
- the variables and parameters defined in the function.
- other internal details

when a function is invoked its execution context gets placed on the execution context stack.
This is a FIFO data structure that stores the execution contexts of the currently running functions.

When a nested function call occurs:
- The execution of the current function is suspended
- The execution context of the new function is created an placed on the stack
- When the function completes its execution context is popped of the stack
- The calling function resumes its execution

any recursive code can be written as a loop.
the loop version is usually more performant 
the recursive version better maps on to the problem domain and is therefore more readable

# Recursive Traversals
---
A recursive data structure is a data structure that is partially composed of smaller or simpler instances of the same data structure.
Recursion is particularly suited to traversing recursively defined data-structures.
`HTML` or `XML` documents, binary trees e.t.c

example

company object is a recursively defined data structure:
- the company has departments (`sales`, `development`)
- each department is either an array of people or another company object with its own departments (`sites`, `internals`)

```javascript
let company = { // the same object, compressed for brevity
  sales: [{name: 'John', salary: 1000}, {name: 'Alice', salary: 1600 }],
  development: {
    sites: [{name: 'Peter', salary: 2000}, {name: 'Alex', salary: 1800 }],
    internals: [{name: 'Jack', salary: 1300}]
  }
};

// The function to do the job
function sumSalaries(dept) {
  let sum = 0;

  for(const [, value] of Object.entries(dept)){
    if(Array.isArray(value)) sum += value.reduce((p, v) => p + v.salary, 0);
    else sum += sumSalaries(value);
  }

  return sum;
}

console.log(sumSalaries(company)); // 7700
```

[linked lists](https://en.wikipedia.org/wiki/Linked_list)
A link list is a recursively defined data structure as well
each item in the list has a `value` and a `next` which points to another item and so on
the last item has `null` as its next value
they allow for `O(n)` splitting, joining, insertion and deletion.

