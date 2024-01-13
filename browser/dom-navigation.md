---
tags:
  - javascript
  - jsinfo
  - frontend
  - draft
source: https://javascript.info/dom-navigation
---
the html document is represented as a tree with each node corresponding to some entity in the document. (see last note).
each node provides various attributes and methods to navigate through the tree.

> [!summary]
> - top level navigation
>   - the document node is the root of the DOM tree.  
>   - `document` holds a reference to the document node.  
>   - `document.documentElement` holds a reference to `<html>`
>   - `document.body` and `document.head` hold references to `<body>` and `head` respectively
> - all node type navigation
>   - `node.childNodes` returns a `NodeList` of references to all child nodes of `node`
>   - `node.firstChild` and `node.lastChild`
>   - `node.parentNode` (element node unless referencing root node)
>   - `node.previousSibling` and `node.nextSibling`
> - element node navigation
>   - `node.children` returns a `HTMLCollection` referencing only child element nodes
>   - `node.firstElementChild` and `node.lastElementChild`
>   - `node.previousElementSibling` and `node.lastElementSibling`
>   - `node.parentElement` (`null` when `node.parentNode === document`)
> - node specific navigation attributes e.g. `<table>` 

note:
  when the browser receives the html from the server it parses the html and builds the DOM tree.  
  the DOM tree is accessible from JavaScript at any point during the parsing of the DOM.  
  code accessing the DOM only has access to the portion of the DOM that has already been built. 
  nodes in the html code that have not yet been parsed by the DOM are not reflected when the DOM is accessed

***navigating the tree***

`document` references the root document node.
`<!DOCType html>` and `<html>` are the children of `document`
`document.documentElement` is a reference to the `<html>` node

`<body>` `<head>` are children of `<html>`
`document.body` holds a reference to `<body>`
`document.head` holds a reference to `<head>`

note:
  if `<body>` or `<head>` are referenced before being built in DOM their values are `null`

`node.childNodes` stores a collection of the child nodes of `node`.
this includes all types of nodes (text, comment, element); even the `\n` text nodes between nodes
`node.firstChild` and `node.lastChild` return the first and last child nodes of `node`.

```html
<!DOCTYPE html>
<!-- hello -->
<html lang="en">
<head>
  <title>Document</title>
</head>
<body>
  <!-- hello world -->
  <h1>Hello World</h1>
  <p>
    Lorem ipsum dolor sit amet, consectetur adipisicing elit. 
    Quae temporibus perferendis, repellendus quod enim tempore voluptate 
    aliquid quaerat illo neque ullam quasi. Totam animi sint odio beatae 
    vitae repellat eos.
  </p>
  <script src="index.js">
  console.log(document.body.childNodes);
  // [ #text, <!-- hello world -->, #text, h1, #text, p, #text, script ]
  </script>
</body>
</html>
```

```javascript
console.log(document.body.childNodes);
// [ #text, <!-- hello world -->, #text, h1, #text, p, #text, script ]
```

`childNodes`
child nodes returns an instance of `NodeList` that is a type of DOM collection.  
`Nodelist` is a read only array-like iterable.  
DOM collections are live in that changes to the DOM are reflected in the collection

note:
  `Array` methods are unavailable on `node.childNodes` as it does not inherit from `Array.prototype`

note:
  `node.childNodes` reflect only the portion of the DOM tree that is built.

`node.parentNode` holds a reference to the immediate parent node of `node`
`node.nextSibling` and `node.previousSibling` store the next and previous siblings of `node`

```javascript
const child = document.body.childNodes;
console.log(child[0].nextSibling === child[1]); // true
console.log(child[0] === child[1].previousSibling); // true
```

***element only navigation***

the previously covered attributes cover navigation between all nodes (text, comment, element)
nodes in the DOM tree also provide attributes to navigate solely between element nodes

`node.children`
returns a `HTMLCollection` array-like, iterable containing references to all child elements of `node`

`node.firstElementChild`
`node.LastElementChild`
`node.previousElementSibling`
`node.nextElementSibling`
`node.parentElement`

typically only element nodes have children hence `node.parentNode` almost always returns an element. however the root of the DOM tree is the document node which is of node type `document` not element. hence attempting to access this node via `document.documentElement.parentElement` returns `null` but `documemt.documentElement.parentNode` returns the `document` node

for convenience some nodes provide additional navigation attributes tailored to their situations  
one such example are the group element nodes used to make html tables  

`<table>`  
`table.rows` returns a collection of `<tr>` elements of the table  
`table.caption|tHead|tFoot` references the `<caption>`, `<thead>` and `<tfoot>` elements
`table.tBodies` returns a collection of `<tbody>` elements. the length of this collection `1 <` 

`<thead>`, `<tfoot>`, `<tbody>` provide the `rows` property  
`tbody.rows` is a collection of `<tr>` 

`<tr>`  
`tr.cells` collection of `<td>` and `<th>` cells  
`tr.sectionRowIndex` the index of `tr` relative to its enclosing section
`tr.rowIndex` the total number of `tr` inside a table

`<td>` and `<th>`  
`td.cellIndex` the number of cells inside the enclosing `<tr>`


