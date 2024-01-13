---
source: https://javascript.info/modifying-document
tags:
  - haskell
  - frontend
  - jsinfo
  - javascript
---

***creation***
`document.createElement(tag)`  
creates a new element of the given tag

`document.createTextNode(text)`  
creates a text node with the given text

***insertion***

the following methods are used to insert nodes into the DOM in a location relative to a node. if a string is passed a text node is inserted. meaning html strings are escaped

`node.append(...(nodes | string)[])`
`node.prepend(...(nodes | string)[])`
`node.before(...(nodes | string)[])`
`node.after(...(nodes | string)[])`
`node.replaceWith(...(nodes | string)[])`

note:
  these insertion methods remove passed nodes from their original location in the DOM

`element.insertAdjacentHTML(location, html)`
used to insert html into the DOM relative to `element`.

`location` can be one of four values: `beforebegin`, `afterbegin`, `beforeend`, `afterend`,


`element.insertAdjacentText(location, text)` wraps `text` in a text node and inserts it  
`element.insertAdjacentElement(location, elem)` inserts `elem`

`node.remove()` removes `node` from the DOM.

`node.cloneNode(deep)` returns a shallow clone of `node` if `deep = false` and a deep clone (including all descendants) if `deep = true`

```html
<!DOCTYPE html>
<!-- hello -->
<html lang="en">
<head>
  <title>Document</title>
</head>
<body foo="bar" bar="baz">
  goodbye world
  <!-- hello world -->
  <h1>Hello World</h1>
  <div id="test">
    <p id="zero">hello world 0</p>
    <div>
      <p id="one">hello world 1 </p>
    </div>
  </div>
  </p>
  <script src="./index.js"></script>
</body>
</html>
```

```javascript
console.log(test.cloneNode(true).querySelector("#one")); // <p id="one">
console.log(test.cloneNode(false).querySelector("#one")); // null
```

`DocumentFragment`  
special DOM node that serves as a wrapper to which other nodes can be attached.
When the `DocumentFragement` instance is inserted into the DOM the wrapper vanishes and only the contents are added.

***ancient times***

`parentElem.appendChild(node)`  
appends `node` as last child of `parentElem

`parentElem.insertBefore(node, nextSibling)`:
inserts `node` as a child of `parentElement` before `nextSibling`

`parentElem.replaceChild(node, oldChild)`

`parentElem.removeChild(node)`

`document.write(html)`
writes `html` into the page.  
if `document.write` is called while html is being parse the browser inserts the content into the DOM as if it was written in the html. This makes it fast and efficient at adding dynamically generated content to a webpage while the page is loading.

`document.write` calls after the page has loaded completely replaces the original page