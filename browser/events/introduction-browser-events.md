---
source: https://javascript.info/introduction-browser-events
tags:
  - frontend
  - javascript
  - draft
---
An event is a signal that something has occurred.  
All DOM nodes generate events as they are all derived from `EventTarget`

> [!note]- Common Events
> ***mouse*** `click` `contextmenu` `mouse<over|out>` `mouse<up|down>` `mousemove`
> 
> ***keyboard*** `keydown` `keyup`
>
> ***form element*** `submit`  `focus`
> 
> ***document events*** `DOMContentLoaded`
> 
> ***css events*** `transitioned`


***event handlers***
even handlers are callback functions registered on DOM nodes that are invoked when an event is triggered. There are several ways to register a handler (python be damned)

***html attribute***
every event has a corresponding `on<event>` attribute (and there for DOM property) on supported DOM nodes.

the `on<event>` element attribute can be used to specify JavaScript code to be run when `event` is triggered.  when building the corresponding DOM node for the element the string specified as the attribute value is used as the body of the `on<event>` method on the node.

```html
<button id="btn" onclick="console.log('hello world')">click</button>
```

```javascript
console.log(btn.onclick.toString()); 
// function onclick(event){ console.log('hello world') }
```

note: default value of the property is `null` if attribute is not specified

the JavaScript specified has access to the top level scopes of all included scripts.

```javascript
// ./index.js
function greet(){
  console.log("hello world");
}
```

```html
<!-- ... -->
  <button onclick="greet()">click</button>
  <script src="./index.js"></script>
<!-- ... -->
```

this does not apply to script tags with `type="module"` (i.e. modules) which have their top level scopes isolated.

note:
`setAttribute` can be used to set the `on<event>` attribute value. weird but it can be done.

```javascript
btn.setAttribute("onclick", "console.log('set from attribute')")
```

warning
the argument to set attribute is always coerced into a string, ergo the follow won't work

```javascript
function greet(){
  console.log('set from attribute')
}

btn.setAttribute("onclick", greet);
// sets onlick to greet.toString()
// <button onclick="function greet(){ console.log('set from attribute')}"></button>


// therefore dom onclick property becomes
btn.onclick = function(event){
  function greet(){
    console.log('set from attribute');
  }
}

// greet is not invoked when button is clicked, it is only defined.
```

question:
does greet have access to `onclick`'s scoped (i.e. the `event` parameter)

```javascript
function greet(){
  console.log('set from attribute =>', event.target.tagName)
}

btn.setAttribute("onclick", greet + "greet()");
// set from attribute =>  BUTTON
// no fucking way that worked ( x \x)
```

***DOM property***
a handler function can be assigned directly to the DOM property for the corresponding event

```javascript
btn.onclick = function(){
  console.log("hello world")
}
```

note:
when assigning directly to the `on<event>` property (via the attribute or DOM node) basic object mechanics apply. Ergo assigning to the property again overwrites the previous handler. Hence only one handler function can be specified.

```html
  <button onclick="console.log('hello world')">click</button>
```

```javascript
console.log(btn.onclick.toString())
// function onclick(event){ console.log('hello world') }

btn.onclick = function(event){ console.log('goodbye world'); }

console.log(btn.onclick.toString())
// function onclick(event){ console.log('goodbye world') }
```

`this`
In keeping with `this` mechanics (as covered in earlier chapters) the value of `this` inside the handler is the DOM node on which the handler function was called (i.e. the object before the dot)

```javascript
btn.onclick = function(event){
  console.log(this.textContent); // click
}
```

warning
arrow functions take `this` from the lexical environment in which they were defined hence using arrow functions as handlers lead to possibly unwanted situations 

```javascript
const person = {
  textContent: "Jesuseun Jeremiah Olatunde",
  setHander(){
    btn.onclick = event => console.log(this.textContent);
    // "Jesuseun Jeremiah Olatunde"
  }
}

person.setHander();
```


`node.addEventListener(eventName, handler[, options])`  
registers `handler` to be called when `eventName` is triggered on `node`. `addEventListener` allows for the registration of multiple handlers

the `options` object allows for additional configuration.
`options.once: boolean`: if `true` the handler is removed after the first triggering of `eventName`
`options.capture: boolean` sets the phase of event triggering in which `handler` is called
`options.passive: boolean` if `true` `event.prentDefault` is not invoked


`node.removeEventLister(eventName, handler[, options]`  
deregisters `handler` listener for `eventName` on `node`
note:
removal `handler` must be the same object passed into `addEventListener`
the same `options` settings must be used as well

waring
some events do not have corresponding `on<event>` dom node properties and as such can only be registered with `node.addEventListener` e.g `DOMContentLoaded`

***event object***
Every event has a corresponding class from which instances containing information about individual events are created. The `Event` class provides the core the interface that represents an event in the DOM. It is the base class of all event classes.

When an event is triggered an instance of its corresponding event class is created and information about the event stored on that instance. This instance is referred to as the event object. The event object is passed an an argument to the handler functions registered to that event.

```html
<button onclick="console.log(event.constructor.name)">click</button>
<!-- MouseEvent -- >
```

```javascript
btn.onclick = function(event){
  console.log(event.target.outerHTML); // <button id="btn">click</button>
}
```

`handlerEvent`

objects with a `handlerEvent(event){...}` method can be assigned as listeners for events using `addEventListener`. When the event is triggered the `handlerEvent` method of the object is invoked and passed the event

this allows for interesting design patterns.