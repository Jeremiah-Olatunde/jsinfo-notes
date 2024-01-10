---
tags:
  - backend
  - javascript
  - jsinfo
  - frontend
  - draft
source: https://javascript.info/string
---
`string`s are stored internally in [UTF-16](https://en.wikipedia.org/wiki/UTF-16) format.

quotes
`string`s are specified by enclosing the character in single or double quotes or backticks.
there is no distinction between single or double quotes
`string`s enclosing in backticks allow for the embedding of arbitrary expressions (string interpolation)
these expressions are placed inside `${...}` within the string
the expressions are evaluated and the result is coerced into a string and then embedded.
backticks also allow strings to span multiple lines, 

note investigate the formatting of multiline strings
note the difference between splitting a `string` value up into multiple lines during assignments and having a `string` with multiple lines i.e. `\n` 
backticks do the latter. the former can be accomplished by breaking and concatenating strings with `+` and using any of the types of quotes.
manually specifying `\n` in a single or double quoted string is equivalent to a multiline string using backticks 

```javascript
let str1 = "Hello\nWorld"; // two lines using a "newline symbol"

// two lines using a normal newline and backticks
let str2 = `Hello
World`;

alert(str1 == str2); // true
```

backticks also allow for [tagged templates](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#tagged_templates).

special characters 
special character begin with a `\` also called an escape character 
`\n` new line 
`\r` carriage return. old timey stuff
`` \` ``, `\'`,  `\"` escape quotes are used to insert quotes into same quoted strings
`\\` backslash used to insert backslash into a string
`\t` tab

the length of a `string` can be access as a [[primitives-methods|primitive property]] on the `string` value.
note special characters (`\n`) are seen as one character 

characters can be accessed using `str[charIdx]` or `str.at(charIdx)` both are zero indexed.
note `string` primitives are temporarily converted to object there for the `str[charIdx]` syntax is just the [[object#Square bracket syntax|square bracket syntax]] and each char becomes `charIdx: charValue` property. ^insight

the `str.at(charIdx)` supports negative indexing starting from `-1` which corresponds to the last character.
`str[charIdx]` returns undefined for negative indexes (makes sense given [[#^insight]])
strings can also be looped over using a `for..of` loop

`string`s are immutable. attempting to mutate them fails quietly i.e. without any error
new strings must be created from old ones in order to modify strings.
all `string` methods that modify a `string` actually return a new `string`

`str.toUpperCase()` and `str.toLowerCase()` methods do what they say.

Searching for substrings
`str.indexOf(substr, pos)` searches for `substr` starting from the `pos` index and returns the index of the first occurrence of `substr` or `-1` if none. 
A loop can be used to find all occurrences by calling the function again from the position of the last found index.
`str.lastIndexOf(substr, pos)` is identical in function but works backwards.
note if `substr` exists at index `0` then using `str.indexOf(substr, pos)` in an if statement will fail as [[type-conversions#Boolean Conversion]|`0` coerces to `false`]]

`str.includes(substr, pos)` returns a `boolean` indicating the presence of `substr` in `str`.
`str.startsWith(substr)` and `str.endsWith(substr)` are obvious in function 

Getting substrings
`str.slice(start[, end])` returns the substring from index \[`start`, `end`)
if `end` is unspecified or `>` the largest index in `str` then slice goes to the end of `str`
`start` and `end` can be negative (note this does not mean a reverse slice)
if `start` >= `end` returns `""`

`str.substring(start [, end])` returns the substring from index \[`start`, `end`)
negative `start` or `end` value are treated as `0`
if `start` >= `end` they are swapped

`str.substr(start [, length])` returns the substring from `start` with given length
if `length` exceeds the length of `str`, `substr` goes tot the end
`start` may be negative to index from the rear of the string

warning `substr` is described in [Annex B](https://tc39.es/ecma262/#sec-string.prototype.substr) of the specification and is only formally supported by browser hosted JavaScript engines.


comparisons
#incomplete

read later
[MDN article on template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#tagged_templates)
[UTF-16](https://en.wikipedia.org/wiki/UTF-16)
[Annex B](https://tc39.es/ecma262/#sec-string.prototype.substr)