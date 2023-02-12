---
title: "Chaining Object Using Proxy"
description: "What is the need to chain a function?"
date: "2023-02-12T08:55:01.683Z"
categories: []
published: false
---

  
**What is the need to chain a function?**

 todo: image

1.  **Pattern one** would lead to [callback hell](http://callbackhell.com/), resulting in unreadable code.
2.  **Pattern two** would lead to unnecessary variables stored in memory, leading to a slow, unresponsive website.

To avoid these two problems is writing methods that can be chained to the next method in the sequence. The ability to chain methods, enables you to conveniently invoke multiple methods on the same target.

Each of the functions in “Function Chaining” returns the current “Execution Context” or current executing object.

```
const Math = {
  result: 0,
  add: function(a, b) {
    this.result = a + b;
    return this;
  },

  multiply: function(a) {
    this.result = this.result * a;
    return this;
  },
 
  divide: function(a) {
    this.result = this.result / a;
    return this;
  }
}

Math.add(10, 20).multiply(5).divide(4);
```

  
With the help of Proxy we can change multiple properties of the object by chaining.

For example in Ruby with jQuery styling can be chained to update a particular element or apply some animation.

```
$(".menu")
  .css("color", "#fff")
  .data("mode", "light")
  .fadeIn();
```

This will be helpful in terms of transforming the DTO we receive from backend to convert into a desirable display format.

```
const transformProxy = {
  get: (object, property) => {
    return (value) => {
      if (value) {
        object[property] = value;
        return new Proxy(object, styleProxy);
      }
      return object[property];
    }
  }
}

const transformObj = (object) => new Proxy(object, transformProxy);
```

The first aspects to understand about using a Proxy are `handlers` and `traps`. We can create a `handler` to `trap` a series of operations, e.g. `get()`, `set()`, and `apply()`. In essence, we’ll get a chance to intercept those operations on the object we’re wrapping and do with them whatever we want—we can return different values depending on some logic, or simply forward the operation to the original target.

  

We at Groww use the proxies to eliminate lot of product & backend terminologies. Abstracting product terminologies benefits the developer to be more productive and makes it less confusing.

For example, we receive the DTO from backend which has multiple complex terminologies. It can be made human readable format or how we display the information to the users.

```
const order = {
  "displayName": "NIFTY 30 Jun '22 - 15800 Call",
  "symbolIsin": "NIFTY22JUN15800CE",
  "orderStatus": "ACKED",
  "qty": 50,
  "price": 13705,
  "duration": "DAY",
  "exchange": "NSE",
  "orderType": "L",
  "buySell": "B",
  "segment": "FNO",
  "product": "NRML",
}
```

Below example gives one

```
transformObj(order).orderType.buySell.product
```
