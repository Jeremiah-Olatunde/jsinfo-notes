---
tags:
  - frontend
  - javascript
  - jsinfo
  - draft
source: https://javascript.info/bubbling-and-capturing
---
the DOM Events standard describes three phases of event propagations
1. capturing phase
2. target phase
3. bubbling phase

***bubbling***
when an event is triggered on a node in the DOM tree the corresponding handlers on that node for the event are run. After that the event is triggered on the parent node which causes the handlers on the parent node for the event to be run. This process repeats, going up the DOM tree till the event is triggered on the root node of the DOM (i.e. the `document`)

note:
note all events bubble

`event.target`  
the `target` property on the `event` object passes to the handler stores a reference to the initial element the event was triggered on. Via `event.target` ancestor nodes may determine the source node of an event.

recall `this` (`=== event.currentTarget`) refers to the node on which the currently running handler is attached (i.e. the object before the dot). note the difference between `event.target` which is the source node of the event


`event.stopPropagation`   
stops the propagation of an event from continuing up the DOM tree. note that if a handler on a node invokes `stopPropagation` is does prevent the execution of the other handlers attached to that node. To accomplish that `event.stopImmediatePropagation` should be used.

***capturing***  
triggering an event on a node actually initiates the event on the root node of the DOM tree. This event then descends down the tree (*"triggering"* on nodes as it goes) until it reaches the target node. This phase is called the capturing phase. 


Subsequently the event the bubbles back up the DOM tree to the root nude. This phase is called the bubbling phase.

to catch an event on the capturing phase `capture: true` must be set on the options object passed into `addEventListener`.

`removeEventListener` only removes the given handler on the specified phase of event propagation

called `event.stopPropagation` during the capturing phase prevents the bubbling and target phases