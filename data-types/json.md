---
tags:
  - backend
  - draft
  - frontend
  - jsinfo
  - javascript
source: https://javascript.info/json
---
[JSON](https://en.wikipedia.org/wiki/JSON) is a language agnostic standard for the string representation of objects and values.

`JSON.stringify(value[, replacer, space])`

`value`: 
	the value to be encoded
	supported types `Object`, `Array`, `string`, `number`, `boolean`, `null`
	
`replacer`:
	an array of properties to be encoded (applied to nested objects as well)
	a mapping function `(key, value) => value | undefined` (skips property if undefined is returned)

`space`:
	the number of spaces to use for indentation
	the character to use for indentation (default `whitespace`)

returns:
	JSON-encoded string

`JSON` object have their property keys and `string` values surrounded in `""`. (no single quotes)

`JSON.stringify` skips JavaScript-specific object properties
 - methods
 - `symbol` keys and values
 - `undefined` properties

`JSON.stringify` supports deeply nested objects
warning: 
	circular references are not supported
	use the `replace` to skip circular references

the `replacer` function is applied recursively through the whole object structure.
the first call `key: value` is `"": wholeAssObject`
`Array` types have their indexes and values passed i.e `"0": value`


`toJSON`
Objects can implement a `toJSON` method that is called automatically by `JSON.stringify` if present.
example:
	`Date` objects implement toJSON that shows `2000-12-31T23:00:00.000Z`


`JSON.parse(string[, reviver])`

`string`:
	`JSON` encoded string to be parse

`reviver`:
	mapping function `(key, value) => value`


read later

[JSON](https://en.wikipedia.org/wiki/JSON) Wikipedia Article on `JSON`
[JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON) MDN Docs on JSON