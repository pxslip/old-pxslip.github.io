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
- `Array.prototype.forEach`

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

### Arrow Functions ###

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

### Promises ###

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

### Generator Functions ###

### Variable Types ###

### Classes ###

```js

function someClass(someParam) {
  this.someParam = someParam;
}

Object.defineProperty(someClass.prototype, 'someParam', {
  get: function() { return this.someParam; },
});

someClass.prototype.logSomeParam = function () {
  console.log(this.someParam);
};

class someClass {
  constructor(someParam) {
    this.someParam = someParam;
  }

  get someParam() {
    return this.someParam;
  }

  logSomeParam() {
    console.log(this.someParam);
  }
}
```

### Modules ###

### Variable Inclusion ###

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

### Async/Await ###

```js
someFunction() {
  let finalResult;
  doSomethingThatReturnsAPromise(param)
    .then(result => {
      finalResult = result;
    });
}

// ES2017
async someFunction() {
  let finalResult = await doSomethingThatReturnsAPromise(param);
}
```

### for...of loops ###

```js
// generator function!
function *fibonacci(n) {
  const infinite = !n && n !== 0;
  let current = 0;
  let next = 1;
  
  while (infinite || n--) {
    yield current;
    [current, next] = [next, current + next]; // destructuring!
  }
}

for (let value of fibonacci(10)) {
  // will log the first 10 fibonacci sequence values
  console.log(value);
}
```

### for...in loops ###

[MDN - for...in loops](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in)

```js
const obj = {
  a: 1,
  b: 2,
  c: 3
};

for (const prop in obj) {
  console.log(`${prop}: ${obj[prop]}`);
}
```

Some caution must be taken using a `for..in` loop as it will loop over all enumerable properties on an array, including inherited ones. Use `hasOwnProperty` to filter prototypical properties.

```js
const parent = {
  a: 1,
  b: 2,
  c: 3
};

function child() {
  this.d = 4;
}

child.prototype = parent;

const childObj = new child();

for (const prop in childObj) {
  console.log(`${prop}: ${childObj[prop]}`);
}
/*
 * Will display:
 * a: 1
 * b: 2
 * c: 3
 * d: 4
 */

for (const prop in childObj) {
  if (childObj.hasOwnProperty(prop)) {
    console.log(`${prop}: ${obj[prop]}`);
  }
}
/*
 * Will display:
 * d: 4
 */
```

### Array.prototype.forEach ###

Some downsides to this are that there is no way to break from the loop like you might with a traditional `for` loop, or `for...of` loop

```js
const values = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
values.forEach(value => {
  console.log(value);
});
```
