---
source: https://javascript.info/browser-environment
tags:
  - backend
  - draft
  - frontend
  - jsinfo
  - javascript
---
JavaScript can be run in a multitude of environments each on a specific platform.
These environments are called host environments and they provide platform specific functionality.
This is typically accomplished through methods and properties on the global object.

The global object on the browser is `window`.
- it acts as a global scope where values can be stored an accessed from every scope
- it represents the browser window and provides methods to control it

***The DOM***
The document object model represents the contents of a webpage as an object.
This object can be modified and the changes are reflected on the page.
the document object can be accessed via `window.document` 


***The BOM***
the browser object model represents additional objects provided by the browser host platform for working with everything aside from the document.  
These objects are also stored as properties `window` global object just like  `document`


read later
[HTML Living Standard](https://html.spec.whatwg.org/)
[DOM Living Standard](https://dom.spec.whatwg.org/)

