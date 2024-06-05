---
source: https://javascript.info/regexp-anchors
tags:
  - javascript
  - jsinfo
  - draft
---
`^` and `$` are called anchor.  
`^` matches the beginning of the string.   
`$` matches the end of the string.  

> [!note] Zero Width
> anchors have zero width. they do not match any character but rather enforce the position of  a pattern

>[!tip]
> surround a pattern with `^...$` to force a full exact match
```typescript
const timeRegExp = /^\d\d:\d\d:\d\d$/;
assert(timeRegExp.test("12:34:00"));
```

