---
source: https://javascript.info/regexp-introduction
tags:
  - javascript
  - jsinfo
  - draft
---
A regular expression consists of a pattern and flags.

there are two syntaxes in javascript  

```typescript
// long syntax
// ideal for dynamically generated regular expression patterns
const regex = new RegExp("pattern", "flags")

// short syntax
const regex = /pattern/gmi
```

  Flags affect the nature of the search.`i|g|m|s|u|y`

`String.prototype.match(regexp)`  
returns an array of all matches if `g` flag.

returns a `RegExpMatchArray` if `g` flag is not specified.
extends `Array` constructor. adds the properties `index`, `input` and `groups`.

if there are no matches `null` is returned. regardless of whether or not `g` flag was specified. i.e an empty array is not returned.

> [!tip] use `?? []`  to always return an array


`String.prototype.replace(regexp, replacement)`

replaces the first match if `g` isn't specified otherwise replaces all matches.

>[!note] 
>replacement can have special symbols specified within it that alter how fragments are put in the replacement string


`RegExp.prototype.test(string)`
Returns true of false if `< 0` matches are found within `string`