---
tags:
  - frontend
  - draft
  - javascript
  - jsinfo
source: https://javascript.info/dom-attributes-and-properties
---
the browser parses the html document to create the DOM tree. when the browser reads a tag or comment or element it creates the corresponding node object (instances of one of the base classes discussed earlier) to place into the tree. 

Standard attributes on html tags become properties of the corresponding object nodes, accessible via the standard dot or square bracket notion. This does not apply to non-standard attributes which are `undefined` if attempted to be accessed in this manner

note:
  standard attributes are described in the [specification](https://html.spec.whatwg.org)

element nodes provide methods for accessing and manipulating attributes. these methods operate on what is written in the html source and hence work on both standard and non-standard attributes

`element.hasAttribute(name)`
`element.getAttribute(name)` returns `null` if attribute does not exist
`element.setAttribute(name, value)`
`element.removeAttribute(name)`

`element.attributes` returns an instance of `NamedNodeMap` which this stores a collection of `Attr` objects.

`NamedNodeMap` is an array like these are stores at numeric index properties on the `NamedNodeMap` instances. however the attributes names are also used as keys in `NamedNodeMap` with the corresponding `Attr` object as values.

```html
<p id="p" foo="bar">
  Lorem ipsum dolor sit amet, consectetur adipisicing elit.
</p>
```

```javascript
console.log(p.attributes[1]); // foo="bar"
console.log(p.attributes["foo"]); // foo="bar"
```

`Attr` inherits directly from `Node`. `Attr.prototype.__proto__ === Node.prototype`. As such `Attr` instances have all `Node` related attributes. the value of an attributes is accessible via the `textContent` or `nodeValue` just like with text or comment nodes. `Attr` objects also have a `name` and `value` property to access the name and value of the corresponding attribute.

note properties of `Attr` instances are stored on their prototype not that instances themselves

```javascript
console.log(Object.getOwnPropertyDescriptors(p.attributes["foo"].__proto__));
```

almost all are accessor properties.

`textContent`, `nodeValue`, `value` have setters and can be used to modify the attribute's value.
the `name` lacks a setter and as such can not be modified.

note:
in

note:
  html attribute names are ***case-insensitive*** and can be accessed via any case
  values assigned to attributes are coerced to strings

***synchronization***
when a standard attributes changes the property on its corresponding object is auto-updated

```javascript
console.log(p.attributes["foo"].nodeValue = [0, 1, 2]);
console.log(p.outerHTML); // <p id="p" foo="0,1,2,3">
```

there are exceptions  
`input.value` only synchronizes from attribute to property.  
setting the `value` attribute via `input.setAttribute(value, text)` sets `input.value = text`
setting `input.value = text` does no affect `value` attribute `input.getAttribute` remains the same.

note that the text displayed in an input element always corresponds to the `input.value` property and not the `value` attribute. Hence the `value` attribute and the text in the text field can differ at certain points

```javascript
// setting value attribute
input.setAttribute("value", "hello world");

// value property updated
console.log(input.value); // "hello world";

// hence text in box updated to match value property


// setting value property
input.value = "goodbye world";

// attribute not updated
console.log(input.getAttribute("value")); // hello world still

// goodbye world displayed in input element box

// value property corresponding to what is visually in the box
// this is independent from value attribute
```

***properties are typed***
attribute values are always of type `string`. however their corresponding properties can be of any type.

`style` attribute is a string but the `element.style` property is an object `CSSStyleDeclaration`

`hidden` property is `true` if the element is hidden and `false` otherwise. 
values assigned to `element.hidden` are coerced into Boolean.

```javascript
p.hidden = 0; // false
p.hidden = ""; // false
p.hidden = "0"; // true non-empty strings are always true
p.hidden = []; // true
```

however values assigned to `hidden` attribute are coerced to `string`.
the attribute value can be set to any `string`.
any string at all makes `element.hidden = true` and hides the element

```javascript
p.setAttribute("hidden", "");
p.hidden; // true
p.getAttribute("hidden"); // ""

p.setAttribute("hidden", [1,2,3]);
p.hidden; // true
p.getAttribute("hidden"); // "1,2,3"
```

in some instances both the property and attribute can be of type `string` but have different values.
`href` property is always a full URL but the attribute can contain a relative URL or just a hash

***custom attributes***  
they can be used to pass data from html to JS, or to style elements. 
custom attributes are generally used to mark elements in a unique manner to implement some from of custom logic.  
custom attributes are easier to access or mutate than classes or ids.  

to avoid custom attributes conflicting with possible future attributes the html standard reserves the `data-*` namespace for developers to use for their custom attributes.

custom attributes with the `data-` prefix are stored in the `element.dataset` property.  
multiword data attributes are converted to camel case `data-foo-bar-baz` becomes `element.dataset.fooBarBaz`.