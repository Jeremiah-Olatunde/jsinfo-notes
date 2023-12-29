---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/number
---

modern javascript has two numeric types
1. `number`: double precision floating point numbers( 64bit [IEEE-754](https://en.wikipedia.org/wiki/IEEE_754))
2. `bigint`: arbitrary length integers `<= -(2**53 - 1)` or `>= (2**53 - 1)`

way to write a number
- the underscore to separate digits similar to the comma in conventional writing.
- scientific notation `xeN` which translates to `x * (10 ** e`)  (note e can be negative).
- prepending `0x`, `0b`, `0o` for hexadecimal, binary or octal numbers respectively.

toString
`num.toString(base)` returns a string representation of `num` in `base` number system.
`2 <= base <= 36`. 
note base 36 is the max as it uses all letter of the alphabet (26 letter + 10 digits)
base 36 can be use to turn long numeric identifiers into short strings

warning 
two syntax required to call a method on a *"raw"* number
or brackets or store in variable

rounding
`Math.floor(num)`: nearest integer `<= num`
`Math.ceil(num)`: nearest integer `>= num`
`Math.round(num)`: nearest integer (`.5` rounds up)
`Math.trunc(num)`: Integer portion of `num`

note multiply-divide to round to the `n-th` digit

`num.toFixed(n)` returns a string that is `num` rounded to `n-th` digit
`num` is padded with 0s if it is to short
note [[operators#Numeric Conversion|unary plus]] to convert `string` to `number`

imprecise calcs
`number`s are represented in 64-bit format
- 52 bits for the digits
- 11 bits for decimal point position
- 1 bit for the sign

If a number overflows the storage it becomes `Infinity`

binary number systems can not represent `0.1` or `0.3` accurately. 
[IEEE-754](https://en.wikipedia.org/wiki/IEEE_754) *"solves"* this by rounding to the nearest binary fraction.
hence `0.1 + 0.2 === 0.30000000000000004` and not `0.3`

solutions include rounding to `n` digits reducing precision requirements
precision error can be decreased by converting fractions to integers (multiplication by `10**n`) performing calculation and then converting them back to fractions

note `9999999999999999` rounds to  `10000000000000000` due to loss of precision error.
52 bits are used to store digits, if the digit requires more the least significant bits are cut off
however using `bigint` by appending `n` resolves this

question how does a number become infinity and when does it round up.

warning due to the manner in which [IEEE-754](https://en.wikipedia.org/wiki/IEEE_754) format represents `number`s `+0` and `-0` are not technically equal. Operators are however suited to treat the as equal. `Object.is` distinguishes them

`isNaN(value)` says what it does
warn  `isNaN` [[type-conversions#Numeric Conversions|numeric conversion]] its argument into a `number` type: `isNaN("hello") === true`
`===` can not be used to check for `NaN` as [[types#`number`|returns `false` for all comparisons]]

`isFinite(value)` checks for `-Infinity` or `+Infinity` or `NaN` after converting `num` to a `number`

tip `isFinite` can be used to check whether a string is  regular number. `"Infinity"` converts to `Infinity` which is a number 

note `Number.isFinite` and `Number.isNaN` do not coerce their arguments. They are the strict equality versions in that sense.

Note `Object.is(value0, value1)` is similar to `value0 === value1` except in a few edge cases
`Object.is(NaN, NaN)` is true. `Object.is(0, -0)` is false.


`parseInt(string, radix)` and `parseFloat(string, radix)` read a number from `string` character by character until a non-numeric character is encountered, in which case they return the read number as an integer or float in base `radix` respectively . 
if no number could be read i.e first characted is non-numeric `NaN` is returned.

`Math.random()`: Random number between `0 <= x < 1`
`Math.max(x0, ..., xN)`: returns largest argument
`Math.min(x0, ..., xN)`: returns smallest argument
`Math.pow(n, power)`: `n ** power`

read later 
[IEEE 745](https://en.wikipedia.org/wiki/IEEE_754)
[Math](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)
