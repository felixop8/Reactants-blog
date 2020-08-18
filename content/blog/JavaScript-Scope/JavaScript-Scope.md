---
title: "JavaScript Fundamentals : Scope"
date: "2020-08-17T22:40:32.169Z"
description: "JavaScript Scope Explained in Simple Words"
author: Félix Oporto López
tags: [javascript, fundamentals, scope]
---

## What is Scope?

<div style={{ padding: '20px', backgroundColor: '#fbe384', marginBottom: '20px', borderRadius: '5px' }}>
  Set of rules for storing variables, functions and objects in a particular part of your code and for finding those variables at a later time.
</div>

The word **Scope** is also used to indicate areas in your code where variables, functions and objects are defined, you can think of these areas as bubbles containing other bubbles — Scope mental map.

We are going to take a look to the different types of scopes(bubbles) existing in our code before moving to the set of rules for storing and accessing variables functions and objects in JavaScript.

There are two types of scopes(bubbles) in JavaScript:
<ul style={{ paddingLeft: '40px'}}>
    <li>Global Scope</li>
    <li>Local Scope</li>
</ul>


## Global Scope

The Global Scope is the area outside all the functions in your code. There is only one Global Scope in your JavaScript document.

```javascript
// global scope
var vegetable = 'carrot';

function foo() {
    // NO global scope
    console.log('Hello World');
}
```

## Local Scope

When variables, functions and objects are declared inside of a function, the area where they are located becomes the **Local Scope**.   
Every function can have its own Local Scope, and it is possible to declare the same variable name in different local scopes with different
values since each variable is bound to the respective scope and are not mutual visible.

```javascript
// global scope

function foo1() {
    // local scope

    var vegetable = 'pepper';

    function foo2() {
        // Local Scope

        var vegetable = 'cucumber';
    }
}

function foo3() {
    // Local Scope

    var vegetable = 'artichoke';
}
```

## What other structures beside functions create bubbles of scope?

### try/catch

The variable declaration in the catch clause to be block-scoped.

```javascript
try {

     undefined() // illegal operation to force an exception!

} catch (err) {

     var b = 2
     var c = err;
     console.log(err); // TypeError: undefined is not a function

}

console.log(err) // ReferenceError
console.log(b); // 3
console.log(c); // TypeError: undefined is not a function
```

### let, const — ECMA6

These two keywords support the declaration of local scope inside of block statements - `if`, `switch`, `for`, `while`, etc.

```javascript
// global scope
{

    let vegetable2 = 'zucchini'; // block scope

    const vegetable1 = 'eggplant'; // block scope

    var vegetable3 = 'tomato';  // global scope
   
}
```

<div style={{ padding: '20px', backgroundColor: '#aad4db', margin: '30px 0 50px 0', borderRadius: '5px' }}>
    I will explain the differences between these three keywords - let, const and var - in a future tutorial. 🤞
</div>


### Garbage Collection

A reason block-scoping is useful (related to closures) is to reclaim memory.


 ```javascript
 function process(data) {
   // do something interesting
}

// anything declared inside this block can go away after!
{
    let someReallyBigData = {...};
    process(someReallyBigData);
}

var btn = document.getElementByID('my_button');
btn.addEventListener ('click', function click(evt) {
    console.log('button clicked');
});
 ```

 The click handler callback doesn’t need the someReallyBigData variable at all. That means, after process(...) runs, the big memory-heavy data structure could be garbage collected. However, it’s quite likely that the JS engine will still have to keep the structure around, since the click function has a closure over the entire scope.

Block-scopin can address this concern, making it clearer to the engine that it doesn’t need to keep someReallyBidData around.


---


## Lexical Scope - Set of Rules

We previously described Scope as a set of rules for storing variables, functions and objects in a particular part of your code and for finding those variables at a later time.

JavaScript uses **Lexical Scope**, which defines a set of rules based on the **physical location** of 
variables, functions and objects in the source code.

Let's examine these rules:

<div style={{ padding: '20px', backgroundColor: '#fbe384', marginBottom: '20px', borderRadius: '5px' }}>
    Children scopes can access variables, functions and objects defined in their parent scope collection.
</div>

```javascript
// parent scope (global scope)

var a = 'Hello World';

function foo() {
    // children scope (local scope)

    console.log(a);
}

foo() // output: Hello World
```

A slightly more complex example:

```javascript
// Global Scope

function foo() {
     // Parent scope of bar() and baz()
    // Children scope of global scope
   

    var a = 'Hello World';

    function bar() {
        // Children scope of foo()

        console.log(a);
    }

    function baz() {
        // children scope of foo()

        bar()
    }

    return baz();
}

foo() // output: Hello World
```

<div style={{ padding: '20px', backgroundColor: '#fbe384', margin: '50px 0 0 0' , borderRadius: '5px' }}>
    Parent scopes <strong>CANNOT</strong> access variables, functions and objects defined in their children scope collection.
</div>

```javascript
// global scope

function foo() {
    // local scope

    var a = 'Hello World';
}

console.log(a); // ReferenceError 'a' is not defined.
```
---

<div style={{ padding: '20px', backgroundColor: '#aad4db', margin: '30px 0 50px 0', borderRadius: '5px' }}>
    ✋One last note, many developers confuse scope with context thinking it's the same thing. But it's not! Scope is all we talked above while Context is used to refer to the value of `this` in some particular parts of the source code. I am going to dedicate an entire tutorial post to talk about `this`, until then just try not to confuse these two distinct concepts.
</div>

