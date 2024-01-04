---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/mixins
---
JavaScript only support single inheritance
An object can only have one `[[Prototype]]`
If the methods of one class are desired on another but inheritance does not make semantic sense the mixin pattern can be used.
A mixin is a class which contains methods that can be used by other classes without those classes having to inherit from the mixin

The mixin pattern is implemented by simply copying the methods from the mixin class' prototype property to the prototype property of the target class

```javascript
function mix(base, mixin){
	// flags aware copying
  Object.defineProperties(
	  base.prototype, 
	  Object.getOwnPropertyDescriptors(mixin)
	);
}

let sayHiMixin = {
  sayHi() {
    console.log(`Hello ${this.name}`);
  },
  sayBye() {
    console.log(`Bye ${this.name}`);
  }
};

// usage:
class User {
  constructor(name) {
    this.name = name;
  }
}

mix(User, sayHiMixin);


new User("Dude").sayHi();
```

