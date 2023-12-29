---
tags:
  - backend
  - javascript
  - frontend
  - jsinfo
source: https://javascript.info/garbage-collection
banner: "![[RDT_20211230_2339317963621244823273346.jpg]]"
---

# Garbage collection

Unlike other languages like C, C++ or Rust, memory management in JavaScript is handled automatically via the *garbage collector*. The garbage collector is responsible for clearing and reallocating memory used by the programming but it no longer referenced.

---
# Reachability

Reachability is the core concept of memory management in object oriented languages. An object is *"reachable"* if it can be accessed within the currently executing scope. These reachable objects are guaranteed to be stored in memory.

1. There exist a base set of values that are always reachable: these are called ***roots***. These can be:
	1. the currently executing function, its local variables and parameters
	2. other function in the chain of nested calls, their local variables and parameters
	3. global variables
	4. other internals 
	
2. Any other value is considered reachable if it is reachable from a root by reference or a chain of references.

> [!note] Object references and copying
> ![[object-copy#Object references and copying]]

> [!example]- Single reference
>Here the object has only a single reference. When that reference is cleared by assigning `null` the object becomes unreachable.
> ```javascript
> 
> // user variable stores a reference to the Jesuseun object
> const user = { name: "Jesuseun" }; 
>
> // the reference is overwritten by null making then Jesuseun unreachable
> const user = null;
>
>// The unreachable object is cleared from memory by the garbage collector
>```

> [!example]- Double reference
> The object has two references. Both must be cleared for the object to become unreachable
> ```javascript
> // both user and admin store references to the Jesuseun object
> const user = { name: "Jesuseun" };
> const admin = user;
> 
> // when user is cleared Jesuseun is still reachable via admin and thus is not 
> // garbage collected
> user = null;
> 
> // still accessible in currently executing scope
> console.log(admin);
> ```

---
# Internal algorithms

The garbage collector monitors all objects and deletes unreachable ones from memory.
This is accomplished using the [mark and sweep algorithm](https://en.wikipedia.org/wiki/Tracing_garbage_collection): 
1. All roots are marked.
2. The references that these roots point to are visited and marked.
3. The references that those references point to are also visited and marked.
4. Step 2 and 3 are repeated until all reachable objects are marked.
5. All unmarked objects are the end of the process are deleted.

JavaScript engines perform multiple forms of optimizations to the basic algorithm outlined here.

---
# Further Reading
1. [A tour of v8](https://jayconrod.com/posts/51/a-tour-of-v8-full-compiler)
2. [Tracing Garbage Collection](https://en.wikipedia.org/wiki/Tracing_garbage_collection)

