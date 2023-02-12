---
title: "Meta Programming in Javascript — 1"
description: "When I first learnt Ruby, I was amazed by the simplicity and developer productivity. Then, I dive deep into it to understand what made Ruby…"
date: "2023-02-12T08:55:01.717Z"
categories: []
published: false
---

  

When I first learnt Ruby, I was amazed by the simplicity and developer productivity. Then, I dive deep into it to understand what made Ruby so loveable to me, and that’s how I was introduced to Meta Programming. So, it made me to explore same concepts in Javascript as well. Thankfully, in ECMAScript 2015, Javascript started to support _Proxy_ & _Reflect_ objects.

Meta Programming by definition, is a technique by which you can write code that _writes_ code by itself. Meta programming could mean a lot. From a simple transformation to intercept certain low level operations / customise the behaviour at the meta level to monkey patching. It can often be misused in a large codebase. So, I believe any meta program should be separated out as a module from the business logic. In this article, will try to give some real world examples that I have personally used and everyone could relate to it and make use of it in your javascript projects.

_Proxy_ & _Reflect_ objects can be used in each major browser except Internet Explorer 11

Traps are internal method detection tools. Whenever you interact with an object, you are calling [an essential internal method](https://www.ecma-international.org/ecma-262/10.0/index.html#sec-invariants-of-the-essential-internal-methods). Proxies allow you to intercept the execution of a given internal method.

  

  

let proxy = new Proxy(target, handler);

  

-   `target` – is an object to wrap.
-   `handler` – is an object that contains methods to control the behaviors of the `target`. The methods inside the `handler` object are called traps.

  

The `get()` trap is fired when a property of the `target` object is accessed via the proxy object.

undefined

The `set()` trap controls behaviour when a property of the `target` object is set.

undefined

  

  

The `handler.apply()` method is a trap for a function call.

  

  

Libraries using Proxy  
[tpyo library](https://github.com/mathiasbynens/tpyo)

```
const array = tpyo(['a', 'b', 'c']);
array.lnegth;
// → `3`

const object = tpyo({
	name: 'Leeroy Jenkins',
	awesome: true
});
object.naem;
// → `'Leeroy Jenkins'`
object.awsum;
// → `true`
```

[Immerj](https://github.com/immerjs/immer)s

Create the next immutable state by mutating the current one

  

  

  

---

If you’ve enjoyed this story, please click the 👏 button and share it, so that others can find it as well! Also, feel free to leave a comment below.

[Groww Engineering](http://tech.groww.in/) publishes technical anecdotes, the latest technologies, and better ways to tackle common programming problems. You can [subscribe here](https://medium.com/groww-engineering) to get the latest updates.

We are hiring. View job openings [here](https://groww.in/careers).
