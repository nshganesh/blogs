---
title: "4 Magical things in React 18"
description: "Automatic Batching"
date: "2023-02-12T08:55:04.024Z"
categories: []
published: false
---

1.  Automatic Batching

React 18: Automatic Batching

The problem with the above code snippet is calling `setValue` and `setResponse` one after another results in two renders. But with the new version 18 of React, it automatically combines both the updates resulting in a single render!

Now the only way to degrade the performance of your React webapp is by using flushSync.

React 18: Not so Automatic Batching

1.  Transitions  
      
    
2.  HTML Streaming
3.  Selective Hydration
