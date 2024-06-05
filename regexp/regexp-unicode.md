---
source: https://javascript.info/regexp-unicode
tags:
  - javascript
  - jsinfo
  - draft
---
Most characters are encoded in two bytes. Some are encoded in 4 bytes.  
Some javascript language features (`.length`) do not correctly process 4 byte unicode characters and instead treat them as 2 unicode characters.  
RegExp by default also experiences this pitfall. however the `u` flag can be specified to avoid this and treat unicode characters properly.

unicode properties `\p{...}`

> [!warning]
> `u` flag must be specified for unicode properties to work


Unicode characters have properties associated with them. these properties describe the category the character belongs to and convey information about the character. `\p{Property}` can be used to match characters that have the specified property. 

e.g `/\p{Letter}/`, `/\p{Number}/` and so on  

>[!note] Short Hands
> - `\p{Lu}` uppercase
> - `\p{Ll}` lowercase
> - `\p{Nd}` decimal digit
> - `\p{Nl}` letter number i.e. Roman Numerals
> - `\p{Nd}` decimal digit
> - `\p{Ps}` punctuation open i.e. `(`, `{`, `[`
> - `\p{Pe}` punctuation close i.e. `)`, `}`, `]`
> - `\p{Sc}` currency symbol

> [!note] Derived Categories
> - `\p{Hex_Digit}` hexadecimal digit
> - `\p{Alpha}` `L` plus `Nl`


all characters and their properties [see](https://util.unicode.org/UnicodeJsps/list-unicodeset.jsp)