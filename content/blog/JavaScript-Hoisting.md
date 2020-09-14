---
title: "JavaScript Fundamentals : Hoisting"
date: "2020-09-14T22:40:32.169Z"
description: "Weird JavaScript hoisting mechanism explained."
author: Félix Oporto López
tags: [javascript, fundamentals, hoisting]
---

I think the best way to explain hoisting is using code examples.

I hope you enjoy reading code 😉


## What is Hoisting?

One way of thinking about hoisting is that **variable** and **function** declarations are moved from where they appear in the flow of the code to the top. It is also important to note that **hoisting is per-scope**.

```jsx
// Before hoisting;

foo();

console.log(b);
var b = 3;


function foo() {
    console.log(a);
    var a = 2;
}
```

```jsx
// After hoisting;

function foo() {
   var a;

   console.log(a); // Undefined; Notice it's not a "ReferenceError: a is not defined".   
   a = 2;
}

var b;


foo();
console.log(b);  // Undefined; Notice it's not a "ReferenceError: a is not defined".
b = 3;
```

### Functions are hoisted first and then variables:

```jsx
// Before hoisting;

foo();
console.log(foo);

var foo = 'bar';
function foo() {
    console.log(1);
};
```
```jsx
// After hoisting;

function foo() {
    console.log(1);
}
var foo; // Duplicated and thus ignored.


foo(); // 1
console.log(foo); // function foo() { onsole.log(1); }.
foo = 'bar';
```

### Functions declarations are hoisted, as we just saw. But *function expressions* and *name function expressions* are NOT

```jsx
// Before hoisting;
foo();
bar();

var foo = function bar() {...}

```
```jsx
// After hoisting;
var foo;

foo(); // TypeError, attempting to invoke an undefined value.
bar(); // TypeError, attempting to invoke an undefined value.

foo = function bar() {...}
```


### While multiple/duplicate var declarations are effectively ignored, subsequent function declarations do override previous one.

```jsx
// Before hoisting.

foo();

function foo() {
   console.log(1);
}


function foo() {
   console.log(2);
}
```

```jsx
// function foo() {
//    console.log(1);
// }

function foo() {
   console.log(2);
}

foo(); // 2
```