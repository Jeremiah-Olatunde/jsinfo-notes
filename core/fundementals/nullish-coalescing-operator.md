---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/nullish-coalescing-operator
banner: "![[titanfall-2-3840x2160-become-one-4k-12402.png]]"
---
# Nullish coalescing operator `??`
---
The `a ?? b` returns `a` if a is not `undefined` or `null` otherwise it returns `b`.  It is essentially syntactic sugar for `(a !== null && a !== undefined) ? a : b`. It is used to provide a default value for a variable if it is either `undefined` or `null`.

> [!note]
> `??` differs from the `||` in that is distinguishes between `falsy` values and `null`/`undefined` values.
>```javascript
> 0 ?? 100 // returns 0
> 0 || 100 // returns 100
>```

The `??` operator and the `||` both have the same precedence. JavaScript throws an error if `??` is used with `&&` or `||`. Parentheses must be used to explicitly specify the desired evaluation order.

> [!tip] 
> Nullish coalescing also supports the modify-in-place syntax.
> ```javascript
>let x = Math.random() < 0.5 ? null : 10;
>x ??= 20; // if x == 10 then it remains 10, if x == null then it becomes 20
>```