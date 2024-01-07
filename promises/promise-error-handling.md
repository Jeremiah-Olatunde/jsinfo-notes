---
tags:
  - backend
  - draft
  - frontend
  - javascript
  - jsinfo
source: https://javascript.info/promise-error-handling
---
the chaining mechanics covered in the previous now allow for elegant error handling techniques when using promises

a single `catch(rejected)` or `then(null, rejected)` can be added to the bottom of a promise chain to catch any error that occurs at any point in the promise chain.

if an error occurs at a certain point in a promise chain it is passed through all consumers until a `catch(rejected)` or `then(null, rejected)` is encountered.

if neither is encountered then a global unhandled rejection error is thrown

***implicit `try...catch`***
as stated earlier if a runtime error occurs within an executor function or within the handlers of any consumers the error is wrapped in a rejecting promise and returned 

***rethrowing***
as mention in the error handling chapter best practice is to only handle very specific errors and rethrow the rest
`catch(rejected)` or `then(null, rejected)` can inspect the error passed into the callback and determine whether or not to handle it and continue with the chain as normal
- either by returning a resolving promise
- or returning a non-promise value which automatically gets wrapped in a resolving promise
or whether to propagate the error down the chain
- by returning a rejecting promise with the error
- or by throwing the error using `throw`

***unhandled rejection***
if an error occur without a consumer to handle it then a global `unhandledrejection` event is triggered  
handlers can be attached to this event using `window.onunhandledrejection`

