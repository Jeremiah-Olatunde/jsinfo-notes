---
tags:
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/styles-and-classes
---
in general there are two ways to style elements using CSS classes or the style attribute.
CSS classes are the preferred method which manipulation of the style attribute through JS is reversed for complex or dynamic modifications.

`element.className`  
is a `string` representing the entire class attribute string. changes to this property reflect changes to the class attribute.

```javascript
console.log(test.className); // "the quick brown fox"

const classes = test.className.split(" ");
classes.splice(classes.findIndex(x => x === "quick"), 1, "slow");

test.className = classes.join(" ");

console.log(test.className); // "the slow brown fox"
```

`element.classList`  
returns an instance of `DOMTokenList` which is an array-like iterable the contains the individual classes as elements of the collection. The whole class string is available via the `value` property which corresponds to `element,className`.

`classList` provides convenience methods for working with the classes in the class string

`element.classList.add(class)`
`element.classList.remove(class)`
`element.classList.contains(class)`
`element.classList.toggle(class)`
`element.classList.replace(class)`

```javascript
console.log(Object.getOwnPropertyDescriptors(test.classList.__proto__));
```

```javascript
console.log(test.className); // "the quick brown fox"
test.classList.replace("quick", "slow");
console.log(test.className); // "the slow brown fox"
```

***resetting***

the read-only `element.style` property is an instance of `CSS2Properties`. 
the properties correspond to CSS style properties.
the keys of these properties are converted from hyphen to camel case
`margin-left` becomes `element.style.marginLeft`.
changing the values of the properties on the `CSS2Properties` object reflects in the DOM

note:
all values are initially empty strings and the styles of the object don't reflect the styles displayed  
`CSS2Properties(0)` reflects that there are `0` set styles??

when setting the value the exact value that would be used in CSS is placed in a string, this includes units and all `element.style.marginRight = "20px"`. 

when a style property is set to an empty string the browser automatically sets the default style
this behavior can be achieved using `element.style.removeProperty(style_property)`  

`element.style.cssText` returns the `string`  value of the `style` attribute and can be used modify the value as a whole.
this has no effect on styles applied from CSS files.


`getComputedStyle(element[, pseudo])`  
returns a `CSS2Properties` instance with all the current styles applied to `element` as properties.
`CSS2Properties(371)`. are there `371` CSS style settings?

A computed style values is the value a style has after all CSS rules and inheritance are applied. such as `height:1em` `font-size:125%`  

A resolved style value is the actual value in `px` applied to the element after the browser performs computations on the relatively specified styles

note:
`:viisted` caveats 