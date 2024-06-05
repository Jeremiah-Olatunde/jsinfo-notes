---
source: https://javascript.info/regexp-character-classes
tags:
  - javascript
  - jsinfo
  - draft
---
Character classes
match any character that is an element of a given set.

`\d`: any single digit
`\s`: spaces i.e. `\n`, `\t`, ` `, `\v`, `\f`, `\r`
`\w`: any letter in the Latin alphabet, digit or underscore

inverse classes
match any character that is not an element of a given set.

`\D`
`\S`
`\W`

Dot  
dot matches any character except `\n`.
the `s` includes `\n` as well

> [!warning]
> `.` does not match the absence of a character
```typescript
const result = "AB".match(/A.B/); // null
```

