---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/alert-prompt-confirm
banner: "![[RDT_20211230_2342115836413625442719996.jpg]]"
---
# Interaction: `alert`, `prompt`, `confirm`
---
 `alert(string)` browser function displays `string` in a modal.
 
 ```javascript
 alert("hello browser");
```

The `prompt(title [,default])` function displays a modal with the `title` text and an input field (optionally prefilled with a default input) and returns the user input as a string or `null` if the `esc` key was pressed.

```javascript
const userInput = prompt("what is your name", "nameless");
```

>[!note]
>`propmt` returns `""` if no input is provided but the `ok` button is pressed.

The `confirm(question)` displays a modal window with `question` and returns `true` if `ok` button is pressed and `false` otherwise.

```javascript
alert(`I am ${ confirm("Are you taller than 6ft") ? "more" : "less" } than 6ft`);
```
