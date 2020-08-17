---
title: "JavaScript Fundamentals : Scope"
date: "2020-08-17T08:55:00.121Z"
description: "JavaScript Scope Explained in Simple Words"
author: Félix Oporto López
tags: [javascript, fundamentals, scope]
---

## What is Scope

<div style={{ padding: '20px', backgroundColor: '#fbe384', marginBottom: '20px' }}>
  Set of rules for storing variables, functions and objects in a particular <strong>scope collection</strong> (global scope, local scope), and for finding those variables at a later time.
</div>


It's fine if you still don't know what a **scope collection** is, I am going to try to explain it right after this, what I want you to take from this definition is that JavaScript has some rules defining how part of your code can access variables, functions and objects depending
on where these are located in the code, and by the way, these rules in JavaScript are part of a set of rules called **Lexical Scope** which are explained after scope collection. 


## Scope collection

### 1.Global Scope

The Global Scope is the area outside all the functions in your code. There is only one.

```javascript
// Start global scope ==========
var vegetable = 'carrot';

function foo() {
    // NO global scope
    console.log('Hello World');
}
// End global scope ==========
```

### 2.Local Scope

When variables, functions and objects are declared inside of a function, the area where they are located becomes the **Local Scope**.   
Every function can have its own Local Scope, and it is possible to declare the same variable name in different local scopes with different
values since each variable is bound to the respective scope and are not mutual visible.

```javascript
// Start  Global Scope ======
function foo1() {
    // Local Scope
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
// End Global Scope ======
```



## Lexical Scope

The set of rules used in JavaScript is called **Lexical Scope**. These rules are based on the **physical location** of 
variables, functions and objects in the source code.

- **First rule**: Children scope collections have access to the variables, functions and objects defined in the parent scope collection. 

```javascript
// Parent scope (global scope)
var a = 'Hello World';

function foo() {
    // children scope (local scope)
    console.log(a);
}

foo() // output: Hello World

```

A slightly more complex example of this rule:

```javascript
// Global Scope
function foo() {
    // Children scope of global scope
    // Parent scope of bar() and baz()
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

- **Second Rule**: Parent scope collections have **NOT** access to the variables, functions and objects defined in the children scope collection.

```javascript
// global scope
function foo() {
    // local scope
    var a = 'Hello World';
}

console.log(a); // ReferenceError 'a' is not defined.
```


## What other structures beside functions create bubbles of scope?

<div style={{ padding: '20px', backgroundColor: '#fbe384', marginBottom: '20px' }}>
  <strong>Block-Scoping</strong>
</div>

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

### let, const 

The let and const keywords attach the variable declaration to the scope of whatever block it’s contained in.

<div style={{ padding: '20px', backgroundColor: '#fbe384', marginBottom: '20px' }}>
  The use of let, const and var is going to be explain in a future tutorial, just keep in mind for now
  that <strong>let</strong> and <strong>const</strong> are special keywords that makes part of a block-scoping to create a its on local scope.
</div>

```javascript
// global scope
if (true) {

    let vegetable2 = 'zucchini'; // block scope
    const vegetable1 = 'eggplant'; // block scope

   
    var vegetable3 = 'tomato';  // global scope
   
}
```

Or just single brackets:

```javascript
// global scope
{
    
    let vegetable2 = 'zucchini'; // block scope
    const vegetable1 = 'eggplant'; // block scope

   
    var vegetable3 = 'tomato';  // global scope

}
```


___

A reason block-scoping is useful (related to closures) is to reclaim memory.


### Garbage Collection
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
