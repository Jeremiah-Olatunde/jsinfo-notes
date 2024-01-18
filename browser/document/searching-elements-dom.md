---
tags:
  - frontend
  - draft
  - javascript
  - jsinfo
source: https://javascript.info/searching-elements-dom
---
`document.getElementById(id)`   
returns the element with an id attribute set to `id`  
if there are multiple elements with the same id any is returned at random  

note:
  the value of the `id` attribute is made a property on the `window` object with the `id` being the key and the element being the value. ergo the element is accessible via the global variable named by `id`. (not recommended)


`elem.querySelectorAll(css_selector)`  
returns a `NodeList` referencing all descendant elements of `elem` matching given css `selector`  
css pseud-classes are supported

`elem.querySelector(css_selector)`  
returns the first element matching the given `css_selector`.  

```javascript
document.querySelector("h1") === document.querySelectorAll("h1")[0]; // true
```

`element.matches(css_selector)`  
returns `true` if `element` matches the given `css_selector` and `false`  otherwise.


`element.closest(css_selector)`  
searches for and returns the nearest ancestor of `element` (starting from `element`) matching the given `css_selector` 


`element.getElementsByTagName(tag)` 
returns a `HTMLCollection` of references to all elements with the given `tag`

`element.getElementsByClassName(class)` 
returns a `HTMLCollection` of references to all elements with the given `class` attribute

`element.getElementsByName(name)` 
returns a `HTMLCollection` of references to all elements with the given `name` attribute 

these methods are all rarely used as `querySelectorAll` is more convenient 

note:
`HTMLCollection` is a live collection and always reflects the current state of the DOM