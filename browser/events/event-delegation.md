---
tags:
  - frontend
  - javascript
  - draft
  - jsinfo
source: https://javascript.info/event-delegation
---
Event delegation is a technique in which a single event handler is attached to a common ancestor node of a group of target nodes. This handler then examines the `event.target` object and determines how to handle the event. This techniques allows for less code and repetition.

```html
  <div class="test-grid" id="test-grid">
    <div class="item"></div>
    <div class="item"></div>
    <div class="item"></div>
    <div class="item"></div>
    <div class="item"></div>
    <div class="item"></div>
    <div class="item"></div>
    <div class="item"></div>
    <div class="item"></div>
  </div>
```

```javascript
const grid = document.getElementById("test-grid");
let curr = null;

grid.addEventListener("click", event => {
  curr?.classList.toggle("highlight");
  curr = event.target;
  event.target.classList.toggle("highlight");
})
```

```css
.highlight {
  background-color: firebrick !important;
}

.test-grid {
  display: grid;
  grid-template-columns: auto auto auto;
  grid-template-rows: auto auto auto;

  height: 500px;
  aspect-ratio: 1;
}

.item {
  background-color: salmon;
  border: 1px solid darkgoldenrod;
}
```

an event might occur not directly on the target nodes but within it i.e. on one of the descendants of these nodes. In that case the `event.target` property will point to the source node and not one of the intended target nodes. In such a situation the `element.closest()` can be used to determine if `event.target` is a descendant of one of the target nodes


***behavior pattern***
Event delegation can be used to implement a multitude of design patterns. One such pattern is the behavior pattern. This goal of the pattern to imbue html elements with *behavior* by simply adding certain data attributes to the elements declaratively 

for example simply adding a `data-toggle-id` attribute to a `<button>` or a `<div>` gives it the *behavior* of toggling the specified id when the element is clicked

```html
<div data-toggle-id="foobar"></div>
<button data-toggle-id="foobar"></button>
```

event delegation can be used to implement this pattern. A click event listener that implements the core id toggling logic is attached to the root node (`document`). This listener inspects the source of the event (`event.target`) for the `data-toggle-id` attribute. If found the specified id is toggled.

Multiple handlers can be added to `document` each with logic to implement different behaviours 


```javascript
document.addEventListener('click', function(event) {
  let id = event.target.dataset.toggleId;
  if (!id) return;

  let elem = document.getElementById(id);

  elem.hidden = !elem.hidden;
});
```