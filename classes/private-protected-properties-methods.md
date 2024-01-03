---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/private-protected-properties-methods
---
private properties are those that are accessible only from within the methods of a class. 
protected properties are those that are accessible from within the methods of a class and classes that are derived from that class.
public properties are those that are accessible from anywhere

private properties are specified by placing a `#` in front of the property 

```javascript
class Person {
	#hiddenAge = 42;

	#hiddenIncrement(){
		this.#hiddenAge += 1;
	}

	window(){
		console.log(this.#hiddenAge);
	}

  stick(){
    this.#hiddenIncrement();
  }
}

class Child extends Person {
  foobar(){
    console.log(this.#hiddenAge); // Error
  }
}


const person = new Person();

person.window(); // 42
person.stick(); 
person.window(); // 43

console.log(person.#hiddenAge); // Error
person.#hiddenIncrement(); // Error
```

private properties allow for the implementation of certain  design patterns.
private properties are intended to be accessed through public getter or setter functions or not at all, serving only as a means of keeping track of some internal state.
if getters and setters are used they usually implement some custom logic that restricts how a private property may be accessed or modified

protected fields are not supported in JavaScript but they are emulated 
properties intended to be accessible only internally are prefixed with an underscore 
this allows them to still be inherited when a class is extended 
note:
	these properties are still technically accessible from external code as there are no language level enforcements preventing this unlike with `#`
	however the general convention is to not do so
	
getter and setter functions for the internal properties maybe created or they can be used to keep track of some internal state.

note:
getters and setter maybe actual accessor properties `get`/`set` or regular methods
accessor properties have the benefit of conciseness but are restricted in that `set` can only take one value

