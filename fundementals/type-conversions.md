---
tags:
  - backend
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/type-conversions
banner: "![[titanfall-2-1920x1080-hd-10479.png]]"
banner_y: 0.60285
---
# Type Conversions
---
Operators and functions convert their input values into values of the required type automatically *(most times)*.

## String Conversions
---
String conversions occur when the string form of a value is required. `alert(value)` for example converts `value` to a string. Manual conversion can be achieved using the `String(value)` function.
String conversions are mostly intuitive. `null` becomes `"null"`, `false` becomes `"false"` etc.

## Numeric Conversions
---
Numeric conversions usually occur in mathematical expressions involving math operators such as  `-`, `/`, `*` or when values are passed to math functions like `Math.abs`, `Math.round` etc. Explicit conversion can be achieved using the `Number(value)` function.

```javascript
"6" / "2" == 3 // true
```

Numeric conversion has a few rules

Value | Results
--- | --- 
`undefined` | `NaN`
`null` | `0`
`false` | `0`
`true` | `1`
`""` | `0`

When converting a string, leading and trailing whitespaces are stripped. If the resulting string is empty `""`, `0` is returned. If there is a failure in conversion `NaN` is returned

## Boolean Conversion
---
Boolean conversion occurs automatically during comparison operations such as those with the logical operators. Manual Boolean conversion is done using the `Boolean(value)` function.

`0`, `null`, `undefined`, `""`, `NaN` become `false` which anything else becomes `true`.

> [!warning] 
> Non-empty string are always true. White space stripping does not occur unlike with `Number`
> ```javascript
>Boolean("0") && Boolean(" ") // true
>```


# Further Reading
---

- [ ] [Type Coercion](https://eishta.medium.com/javascript-tricky-questions-type-coercion-7fe47b4c488e)