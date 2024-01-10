---
tags:
  - backend
  - frontend
  - draft
  - javascript
  - jsinfo
source: https://javascript.info/date
---
`Date` is a built-in object that provides a convenient abstraction for date/time management

note:
	`GMT` is the same as `UTC`

note:
	When an instance of `Date` is made the time is stored relative to `UTC-0`
	methods `getHours` return the time in local time
	`new Date()` gives `2023-12-13T20:41:36.198Z`
	`new Date().getHours()`  (in Nigeria `UTC+1`) gives `21` 

creation syntaxes

```javascript
new Date(); // new date object for current date and time

// timestamp is an integer representing the number of milliseconds
// since Jan 1 1970 UTC+0. can be negative
new Date(timestamp); 

// string representation of target date
// if time is not set is it assumed to be midnight UTC
// the time is adjusted to the local time zone
// e.g. Nigeria UTC+1, date.getHours() === 1
// e.g. Australia UTC+11, date.getHours() === 11
// dummed down when it was midnight at UTC-0 it was 1am in Nigeira
new Date(dateString);

// year - XXXX format
// month - [0, 11] January - December
// date - day of the month. not zero indexed god knows why
new Date(year, month, date = 1, hours = 0, minutes = 0, seconds = 0, ms = 0);
```

accessing date components

`date.getFullYear()`
`date.getMonth()`
`date.getDay()`
`date.getHours()`
`date.getMinutes()`
`date.getSeconds()`
`date.getMilliseconds()`

methods return their values in local time. the date object itself stores `UTC-0` time.
to get raw `UTC` use the `date.getUTCXXX()` versions of the methods above

warning:
	`date.getYear()` returns a 2 (`19XX` years) or 3 (`20XX` years) digit number representing the year
	2001 is `101`, 2017 is `117`, 3001 is `1101`
	it is depreciated in favor of `date.getFullYear()`

`date.getTime()` returns the timestamp of the date. i.e. the number of milliseconds since the Unix Epoch

`date.getTimezoneOffset()` returns the different between `UTC` and the local time zone

setting date components

- [`setFullYear(year, [month], [date])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setFullYear)
- [`setMonth(month, [date])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setMonth)
- [`setDate(date)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setDate)
- [`setHours(hour, [min], [sec], [ms])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setHours)
- [`setMinutes(min, [sec], [ms])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setMinutes)
- [`setSeconds(sec, [ms])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setSeconds)
- [`setMilliseconds(ms)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setMilliseconds)
- [`setTime(milliseconds)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setTime)

these methods set their respective date components based on local time and return the new timestamp
that is `setHours(21)` in Nigeria (`UTC-1`) sets the date object to `20` in `UTC-0`
the `UTC` time can be set with the corresponding `UTC` variants of the methods above
note: 
	setTime does not have a `UTC` variant

Autocorrection

Date objects allow for the setting out of range values that are automatically adjusted
e.g. Setting a date to the 32nd of January is automatically corrected to 1st of February
tip:
	use autocorrection to get the date after a certain period of time
	set zero or negative values `date.setDate(0)` sets date to the last day of the previous month
	months are no zero indexed

the algorithm for [[object-toprimitive|object to primitive]] conversion for `Date` returns the timestamp of the date
i.e. `valueOf()` and `[Symbol.toPrimitive](number)` return `date.getTime()`
tip: 
	convert date to timestamp using unary `+` 

`Date.now()` returns the current timestamp 
tip:
	use for convenience or benchmarking or performance intensive situations 

`Date.parse(str)` parses `str` into its corresponding date returning the timestamp of that date
an invalid format returns `NaN`
`str` should be of the format: 
	`YYYY-MM-DDTHH:mm:ss.sssZ`
	`YYYY-MM-DD` `YYYY-MM` `YYYY` e.t.c.

passing an invalid string format into `new Date(string)` returns an `Invalid Date` object
all calls to accessor methods return `NaN`
setting any of the date components
