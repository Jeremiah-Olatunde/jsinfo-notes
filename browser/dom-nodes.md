---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/dom-nodes
---
html documents are modelled using a tree data structure called the DOM tree.
the hierarchical structure of the html document is modelled using parent and child nodes on a tree.


Every entity in a html document is represented by a corresponding node types.
There are 12 node types but the major ones:
- element node to represent html tags `<html>` `<li>` `<p>`
- comment nodes to represent html comments
- text nodes to represent text. (text within html elements and also text in between html elements too (such as the `\n` and ` ` characters added to give the document visual structure) 
- document node which serves as the entry point into the DOM


The DOM tree is represented as a JavaScript object stored in `window.document`.
the `document` object is the root node of the tree and corresponds to the document node

Each node on the document tree is represented by a corresponding to an object.
child nodes are stored as properties on their parent node's object.

these node objects provide methods for accessing their surrounding nodes (i.e. parent nodes, child nodes, sibling node) in addition to methods and attributes for various other things.
The object representing the html document is called document object `window.document`.  


***autocorrection***
when html documents are improperly written the browser automatically inserts corrections into the document.

note: by specification html tables must have a `<tbody>` tag. if omitted browsers automatically inset one