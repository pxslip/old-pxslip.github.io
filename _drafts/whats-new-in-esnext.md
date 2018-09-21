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
- Trailing commas
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
let param = 'endpoint';
doSomethingThatReturnsAPromise(param)
    .then(url => {
        // look ma an arrow function
        // preserves the scope
        return $.get(url);
    }).then(response => {
        return callSomethingElse(response);
    }).then(() => {
        finishUp();
    });
```

### Generator Functions ###

```js
function* generator(start) {
  let i = 0;
  while (i < 100) {
    yield i;
    i = i + step;
  }
}

let gen = generator(5);
console.log(gen.next().value; // 0
console.log(gen.next().value; // 5
console.log(gen.next().value; // 10
```

### Variable Types ###

[MDN - let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)

[MDN - const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)

```js
// bound to the current execution context
var a = 1;
// bound to the current block scope
let b = 2;
// constant value, cannot be re-assigned, also block scoped like let
const c = [];

if (b === 2) {
  var a = 2;
  let b = 3;
  let d = 4;
  console.log(d); // 4
}
console.log(d); // error

try {
  c = 4;
} catch (err) {
  // throws an error when trying to re-assign to a const
  console.log(err);
}

```

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

```js
// module.js
export default function (someParam) {
  // do something here
}
// uses-module.js
import func from './module';
// maps to the function that was exported
func(1);
```

```js
// module.js
export function doSomething(someParam) {
  // do something here
}

export const someConst = 1;

// uses-module.js
import { doSomething, someConst } from './module';

console.log(someConst);
console.log(doSomething(1));
```

### Template Literals ###

[MDN - Template Literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)

```js
// ES3
var somePart = 'Will'
var someString = 'Hello ' + somePart + '. How are you today?';

// ES2015
let somePart = 'Will';
let someString = html`Hello ${somePart}. How are you today?`;
```

### Function Defaults ###

```js
// old
function doSomething(a, b) {
  a = a || 1;
  b = b || 2;
  // do something
}

// new
function doSomething(a = 1, b = 2) {
  // do something
}
```

### Rest Parameters/Spread Operator ###

[MDN - Rest Parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)

```js
function count(...args) {
  return args.length;
}

console.log(count(1, 2, 3, 4)); // 4
console.log(count(1, 2, 3, 4, 5, 6)); //6
```

[MDN - Spread Syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

```js
function sum(x, y, z) {
  return x + y + z;
}

const numbers = [1, 2, 3];

console.log(sum(...numbers));
// expected output: 6

console.log(sum.apply(null, numbers));
// expected output: 6
```

### Destructuring ###

[MDN - Destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

```js
let a, b, rest;
[a, b] = [10, 20];

console.log(a);
// expected output: 10

console.log(b);
// expected output: 20

[a, b, ...rest] = [10, 20, 30, 40, 50];

console.log(rest);
// expected output: [30,40,50]
```

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

### Accessing parent with `super` ###

[MDN - Super](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/super)

```js
class Rectangle {
  constructor(height, width) {
    this.name = 'Rectangle';
    this.height = height;
    this.width = width;
  }
  sayName() {
    console.log('Hi, I am a ', this.name + '.');
  }
  get area() {
    return this.height * this.width;
  }
  set area(value) {
    this.height = this.width = Math.sqrt(value);
  }
}

class Square extends Rectangle {
  constructor(length) {
    this.height; // ReferenceError, super needs to be called first!

    // Here, it calls the parent class' constructor with lengths
    // provided for the Rectangle's width and height
    super(length, length);

    // Note: In derived classes, super() must be called before you
    // can use 'this'. Leaving this out will cause a reference error.
    this.name = 'Square';
  }
}
```

### Dynamic Properties ###

[MDN - Object Initializer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#New_notations_in_ECMAScript_2015)

```js
let propName = 'foo';

let o = {
  [propName]: 'bar'
};
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

### Map/Set ###

[Maps](https://en.wikipedia.org/wiki/Associative_array)
[MDN - Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

```js
let map = new Map();

map.set('key', 'value');
map.get('key');
```

[Sets](https://en.wikipedia.org/wiki/Set_(abstract_data_type))
[MDN - Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)

```js
let set = new Set();

set.add(1);
set.add('some string');
console.log(set.size); // 2
set.add(1);
console.log(set.size); // 2
```

### Array.prototype.forEach ###

Some downsides to this are that there is no way to break from the loop like you might with a traditional `for` loop, or `for...of` loop

```js
const values = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
values.forEach(value => {
  console.log(value);
});
```

### Array.prototype.includes ###

[MDN - Array.prototype.includes()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)

```js
let arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];

// old
arr.indexOf(1) !== -1; // true
arr.indexOf(100) !== -1; // false

// new
arr.includes(1); // true
arr.includes(100); //false
```

### ** (Exponentiation) ###

```js
let exp;
// old
exp = Math.pow(7, 3); // 343

// new
exp = 7**3;
```

### String.prototype.padStart/String.prototype.padEnd ###

[MDN - padStart](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/padStart)

```js
const str = '5';
str.padStart(5, '0'); // 00005
```

[MDN - padEnd](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/padEnd)

```js
const str = '5';
str.padEnd(5, '0'); // 50000
```

### Object.values ###

[MDN - Object.values()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Object/values)

```js
const someObj = {
  a: 1,
  b: 2,
  c: 3
};

Object.values(someObj); // [1, 2, 3]

```

### Object.entries ###

[MDN - Object.entries](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)

```js
const someObj = {
  a: 1,
  b: 2,
  c: 3
};

Object.entries(someObj); // [['a', 1], ['b', 2], ['c', 3]]
```

### Trailing Commas ###

[MDN - Trailing commas](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Trailing_commas)

```js
const someArray = [
  1,
  2,
  3,
  4,
];

const someObj = {
  a: 1,
  b: 2,
  c: 3,
};

class Z {
  func1() {},
  func2() {},
}

function someFunc(a, b, c,) {
  // do something
}

someFunc(1, 2, 3,);
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

### SharedArrayBuffer/Atomics ###

Interesting discussion to be had around worker threads, and sharing data from the main thread to the worker thread. This introduces the old issue of race conditions, etc. SharedArrayBuffers solve the first, and Atomics the second. See - [ES proposal: Shared memory and atomics](http://2ality.com/2017/01/shared-array-buffer.html).

### Promise.prototype.finally ###

[MDN - Promise.prototype.finally](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally)

```js
let isLoading = true;
fetch(request)
  .then(response => {
    // do something
  })
  .catch(error => {
    //uh oh, that's an error
  })
  .finally(() => {
    // clean up
    isLoading = false;
  });
```

### for await...of ###

```js
// see: http://2ality.com/2016/10/asynchronous-iteration.html
async function f() {
    for await (const x of createAsyncIterable(['a', 'b'])) {
        console.log(x);
    }
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
