---
title: "JavaScript: Arrow Functions Vs Traditional Functions."
description: "Gotchas around arrow functions"
date: "2022-07-05T07:33:27.683Z"
categories: []
published: true
canonical_link: https://medium.com/@nshganesh/javascript-arrow-functions-vs-traditional-functions-c41e3f3dba9d
redirect_from:
  - /javascript-arrow-functions-vs-traditional-functions-c41e3f3dba9d
---

An arrow function expression is a compact & lightweight alternative to a traditional function expression with a focus on being short in syntax.

```
// Traditional Function
function add(a, b) {
  return a + b;
}

// Arrow Function
let add = (a, b) => a + b;
```

There are some distinctions between _arrow functions_ and _traditional functions_. In this blog, I’ll try to cover those as well as some pitfalls to avoid.

Photo by [Zoltan Tasi](https://unsplash.com/@zoltantasi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

---

1.  **Arrow functions don’t have their own bindings to** `**this**`**,** `**arguments**` **or** `**constructors**`**.**

```
// Traditional Function

const Person = function() {
  this.name = 'John';
};
new Person(); // -> Person { 'name': 'John' }

// Arrow Function

const Person = () => {
  this.name = 'John';
};
new Person(); // -> TypeError: Person is not a constructor
```

> When `new Person()`creates a new object, it inherits from `Person.prototype`. However, arrow functions lack a `prototype` property. As a result, arrow functions cannot be used as constructors and will fail when used with `new`.

```
// Traditional Function with arguments

const Person = function() {
  return arguments;
};
Person("John"); // -> { '0': 'John' }

// Arrow Function with arguments

const Person = () => arguments;
Person("John"); // -> Uncaught ReferenceError: arguments is not defined
```

At the same time arrow functions do not provide a binding for the `arguments` object. As an alternative use the `rest parameters` to achieve the same result as traditional function:

```
const Person = (...args) => args;
Person("John");
```

**2**. **Arrow functions don’t have access to the** `**new.target**` **keyword.**

`new.target` is a meta property introduced in javascript for runtime evaluation. Its primary purpose is to provide a better way to detect when a constructor is called without the `new` keyword. When creating javascript modules, this can be used to eliminate the risk of namespace collision.

```
// Traditional way

function Person(name) {
  this.name = name;
}
```

In traditional way we can call Person function in two ways:

1.  using `new` keyword. It will print the variable’s name and keep it in the function execution context.

```
const john = new Person('John');
console.log(john.name); // John
```

2\. Just as a function. By doing so, the variable will be added to the window object.

```
const john = Person('John');
console.log(john) // undefined
console.log(window.name) // John
```

We can use `new.target` to avoid the risk of adding variables to global objects.

```
function Person(name) {
  if (!new.target) {
    throw "must use new operator";    
  }
  
  this.name = name;
}

const person = Person('John'); // throws must use new operator
const person = new Person('John'); // works!
```

Arrow functions, they lack access to `new.target`. We also cannot use the `new` keyword with arrow functions. Using `this` property within arrow functions may result in a namespace collision.

```
const Person = (name) => this.name = name;
const john = Person('John')

console.log(john) // John
console.log(window.name) // John

const john = new Person('John') // throws Person is not a constructor
```

**3**. **Arrow functions aren’t suitable for** `**call**`**,** `**apply**` **and** `**bind**` **methods.**

```
// Traditional Way

// A simplistic object with its very own "this".
var obj = {
  number: 100
}

// Set "number" on window.
window.number = 2000;

// A traditional function to operate on "this"
var add = function (a, b, c) {
  return this.number + a + b + c;
}

// call
var result = add.call(obj, 1, 2, 3) // establish the scope as "obj"
console.log(result) // result 106
```

With arrow functions, since`add` function is essentially created on the global scope, it will assume `this` as the window.

```
// Arrow Example

// A simplistic object with its very own "this".
var obj = {
  number: 100
}

// Set "number" on window.
window.number = 2000;

// Arrow Function
var add = (a, b, c) => this.num + a + b + c;

// call
console.log(add.call(obj, 1, 2, 3)) // result 2006
```

**4\. Arrow functions cannot use** `**yield**` **keyword.**

There has been some debate about whether or not not supporting yield with arrow functions is a design flaw. The proposal for generator arrows, on the other hand, has been moved to Stage 1, and this is the official [repo](https://github.com/tc39/proposal-generator-arrow-functions) with more discussion about syntax and other details.

**5\. Arrow function calls on empty objects.**

```
// Arrow Function on other types

const ten = () => 10;
ten(); // -> 10

// Arrow Function on empty object
const obj = () => {};
obj(); // -> undefined
```

You might expect `{}` instead of `undefined`. This is because the curly braces are part of the syntax of the arrow functions, so `obj`will return `undefined`. It is however possible to return the `{}` object directly from an arrow function, by enclosing the return value with brackets.

```
const obj = () => ({});
obj(); // -> {}
```

---

If you’ve enjoyed this story, please click the 👏 button and share it, so that others can find it as well! Also, feel free to leave a comment below.

[Groww Engineering](http://tech.groww.in/) publishes technical anecdotes, the latest technologies, and better ways to tackle common programming problems. You can [subscribe here](https://medium.com/groww-engineering) to get the latest updates.

We are hiring. View job openings [here](https://groww.in/careers).
