# Understand JavaScript
## Understand 'this' by examples

Example is in node.js environment.
In browser, replace 'global' with 'window'.
```js
const objA = {
  returnArrowFunc: function() { return () => this; },
  returnArrowFuncWithStrict: function() {
    "use strict"
    return () => this;
    },
  returnRegularFunc: function() {
    return function() { return this; } },
  returnRegularFuncWithStrict: function() {
    "use strict"
    return function() { return this }
  },
  returnThisWithStrict: function() {
    "use strict"
    return this;
  }  
}

const referenceToReturnArrowFunc = objA.returnArrowFunc;

const objB = {};

objA.returnArrowFunc()() === objA; // true;
referenceToReturnArrowFunc()() !== objA //true
// Browser returns window here 
// Node doesn't return global (why?)

objA.returnArrowFuncWithStrict()() === objA; // true
// arrow function's [[ThisMode]] == 'lexical' overrides the function's strict/non-strict status.

objA.returnRegularFunc()() == global; // true;
objA.returnRegularFuncWithStrict()() == undefined; // true
objA.returnThisWithStrict() == objA; // true
objA.returnArrowFunc.call(objB)() == objB; //true
objA.returnArrowFunc().call(objB) == objA; //true

// this inside arrow function only cares about what its parent scope gets for its "this". 
objA.returnArrowFuncWithStrict.call(objB)() === objB // true

// Arrow function's "this" is bound permanently with the "this" of it's parent function scope as soon as that function gets invoked.
objA.returnArrowFuncWithStrict().call(objB) === objA // true

objA.returnRegularFunc.call(objB)() == global // true
objA.returnRegularFunc().call(objB) == objB // true
objA.returnRegularFuncWithStrict.call(objB)() == undefined // true
objA.returnRegularFuncWithStrict().call(objB) == objB //true


```
