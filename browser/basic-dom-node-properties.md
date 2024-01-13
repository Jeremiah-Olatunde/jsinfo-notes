---
tags:
  - frontend
  - draft
  - javascript
  - jsinfo
source: https://javascript.info/basic-dom-node-properties
---


![[Pasted image 20240113082215.png]]  

the diagram depicts the inheritance hierarchy graph of nodes in the DOM.

`EventTarget`   
It is the root of the hierarchy. It is an abstract base class the provides event functionality 

`Node` 
It is an abstract base class serving as the base for all DOM nodes.  
it provides all the node specific navigation covered earlier (`parentNode`, `nextSibling` `childNodes`)

`Element` 
it is an abstract base class serving as the base for all DOM elements.
it provides element level navigation (`children`, `parentElement`, `previousElement`)

`HTMLElement`   
it is an abstract base class that all concrete html element classes inherit from.
it provides fundamental behavior for all html elements.  
Other html elements extend this class to implement their own functionality on top of it
these include `HTMLInputElemenet` `HTMLAnchorElement` etc..
elements like `<span>`, `<section>` etc. do no provide any extra functionality and are direct instances of `HTMLElement`


```javascript
function getChain(obj){
  const proto = Object.getPrototypeOf(obj);
  if(proto === null) return ["null"];
  return getChain(proto).concat(proto.constructor.name);
}

console.log(getChain(HTMLAnchorElement.prototype));
// [ "null", "Object", "EventTarget", "Node", "Element", "HTMLElement" ]
```

`node.nodeType` returns a `number` indicating the type of `node`
`1` for element nodes
`3` for text nodes
`9` for document nodes


`node.nodeName`
returns the name of `node`. `H1`, `BODY`, `#comment`, `#document`

`element.TagName`
returns the tag name of `element`. tag name is always in all caps except in xml documents

`element.nodeName === element.tagName`


`element.innerHTML` 
allows the access and manipulation of all the html inside `element` as a `string`

note:
  `element.innerHTML += "new html string"` does a full rewrite of the html in `element`
  this means all resources will be reloaded

note:
  `<script>` tags inserted using `innerHTML` are no executed


`element.outerHTML`  
returns a `string` containing the html inside element and the element itself
changing `outerHTML` replaces the element in the DOM.
however references to the original element are still valid

```javascript
const para = document.getElementById("p");
para.outerHTML = "<div id='p'>hello world</div>";

// element is no longer in DOM
// para still holds a reference to it
console.log(para.tagName); // P 

// inserted div needs to be refetched
const div = document.getElementById("p");
console.log(div.nodeName); // div
```


`node.nodeValue`/`node.data`
both return the data stored within `node`. 
`node.nodeValue` is `null` for element nodes
`node.data` is `undefined` for element nodes 

```javascript
console.log(document.body.childNodes[0].data); // \n goodbye world
console.log(document.body.childNodes[1].data); // hello world

console.log(document.body.data === undefined);
console.log(document.body.nodeValue === null);
```

`node.textContent`
returns all the text content within a the descendant text and comment nodes of `node`.

recall only element nodes have descendants ergo `textContent` of text and comment nodes returns the same string as `node.nodeValue`/`node.data` 

only the text content is returned, all tags are stripped.

```html
<!DOCTYPE html>
<!-- hello -->
<html lang="en">
<head>
  <title>Document</title>
</head>
<body>
  goodbye world
  <!-- hello world -->
  <h1>Hello World</h1>
  <p id="p">
    Lorem ipsum dolor sit amet, consectetur adipisicing elit. 
    Quae temporibus perferendis, repellendus quod enim tempore voluptate 
    aliquid quaerat illo neque ullam quasi. Totam animi sint odio beatae vitae repellat eos.
  </p>
  <script src="./index.js"></script>
</body>
</html>
```

```javascript
console.log(document.body.childNodes[0].textContent); // \n goodbye world
console.log(document.body.childNodes[1].textContent); // hello world
console.log(document.body.textContent);

/*


  goodbye world
  
  Hello World
  
    Lorem ipsum dolor sit amet, consectetur adipisicing elit. 
    Quae temporibus perferendis, repellendus quod enim tempore voluptate 
    aliquid quaerat illo neque ullam quasi. Totam animi sint odio beatae vitae repellat eos.

*/
```


writing to `textContent` provides a safer means of inserting plain text into the DOM.
if html containing `string` is written to `textContent`   it is escaped.  
this is in contrast to `innerHTML` and `outerHTML` that render the passed `string` as html.  


`element.hidden`
is a Boolean value that indicates whether an element is visible or not.  
this is similar to `style="display:none"`





read later
[DOM specificiation](https://dom.spec.whatwg.org/#eventtarget)
[Full list of attribute support by each HTMLElement](https://html.spec.whatwg.org/#htmlinputelement)

