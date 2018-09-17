---
layout: post
title: "What's new in ESNext (ES2015, ES2016, ES2017, ES2018)"
date: 2018-09-14
author: Will Kruse
---
A quick rundown of new features introduced since ES5 came out

## ES2016/ES5 ##

This is the big one, the first release in 6 years it introduced many of the features we will focus on

- Arrow functions
- Promises - as a core language feature
- Generators
- New Variable types
  - `let`
  - `const`
- Classes
- Modules
  - Export
  - Import
- Template literals
  - Tagged template literals
- Function defaults
- Spread Operator
- Rest operator
- Destructured assignments
- Object Enhancements
  - [variable inclusion](#variable-inclusion)
  - prototype definition
  - access to parent as `super`
  - Dynamic Properties
- `for...of` loops
- `Map` and `Set`

## ES2016/ES7 ##

From here on out ES releases a new spec on a yearly basis, meaning smaller, incremental updates

- `Array.prototype.includes`
- `**` exponentiation operator

## ES2017/ES8 ##

- String padding functions
  - `String.padStart`
  - `String.padEnd`
- Object property access
  - `Object.values`
  - `Object.entries`
- `Object.getOwnPropertyDescriptors`
- Trailing commas
  - function calls
  - parameter lists
- Async/Await
  - `async` functions
  - `await` keyword

## ES2018/ES9 ##

This is the latest release, published June 2018.

- Shared memory/Atomics
  - For use with workers
- RegEx Enhancements
  - `.` matches _all_ characters (requires `s` flag)
  - Named capture groups `(?<name>...)`
- Rest/Spread Operator on objects
- `Promise.finally`
- `for await..of`

## Examples ##

### <a href="arrow-functions"></a> Arrow Functions ###

source: [MDN - Arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

```js
// ES3
function Person() {
  var that = this;
  that.age = 0;

  setInterval(function growUp() {
    // The callback refers to the `that` variable of which
    // the value is the expected object.
    that.age++;
  }, 1000);
}
// OR
function Person() {
  this.age = 0;

  setInterval((function growUp() {
    // The value of this was set using the bind method
    this.age++;
  }).bind(this), 1000);
}
// ES2015+
function Person() {
  this.age = 0;

  setInterval(() => {
    // The value of this does not change
    this.age++;
  }, 1000);
}
```

### <a href="promises"></a> Promises ###

See: [MDN - Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

```js
// ES3
doSomethingThatRequiresACallback(param, function (something) {
    // we're in the callback, but what if we need to do something else that needs a callback...
    $.get('somewhere', function (response) {
        // we have the response and want to do something with that
        callSomethingElse(response, function () {
            // ok we're finally done, but where in callback hell are we?
            finishUp();
        })
    });
});

// ES2015+
// assume all functions properly return a promise
doSomethingThatReturnsAPromise(param)
    .then(() => {
        // look ma an arrow function
        // preserves the scope
        return $.get('somewhere');
    }).then(response => {
        return callSomethingElse(response);
    }).then(() => {
        finishUp();
    });
```

### <a href="variable-inclusion"></a> Variable Inclusion ###

```js
// ES3
var someVariable = 'Hello World';
var someObject = {
    someVariable: someVariable
};
// ES2015
const someVariable = 'Hello World';
const someObject = {
    someVariable
};
```
