---
tags:
  - backend
  - frontend
  - draft
  - javascript
  - jsinfo
source: https://javascript.info/arrow-functions
---
arrow functions have no `arguments` (taken from outer lexical environment)
arrow functions have no `this` (taken from outer lexical environment)
arrow functions can not be used with `new`
`this` of arrow functions can not be bound with `bind`

```javascript
let group = {
  title: "Our Group",
  students: ["John", "Pete", "Alice"],

  showList() {
    this.students.forEach(
	    // this in this.title is taking from showList lexical environment
      student => alert(this.title + ': ' + student)
      
      // this would throw and error
      // function(student){ alert(this.title) } 
    );
  }
};

group.showList();
```

```javascript
function defer(f, ms) {
  return function dummy() {
	  // both this and arguments are taking from dummy
    setTimeout(() => f.apply(this, arguments), ms);
  };
}
```


read later
https://en.wikipedia.org/wiki/Currying
https://en.wikipedia.org/wiki/Partial_application
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind
what is the difference between currying and partial functions