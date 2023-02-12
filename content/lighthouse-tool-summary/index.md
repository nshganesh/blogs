---
title: "Lighthouse tool summary"
description: "Audit tool for performance monitoring"
date: "2023-02-12T08:55:10.109Z"
categories: []
published: false
---

  

Audit tool for performance monitoring

1.  Critical rendering path

Critical Request Chains

2\. Deferring unused CSS

Unused CSS also slows down a browser’s construction of the [render tree](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-tree-construction). The render tree is like the DOM tree, except that it also includes the styles for each node. To construct the render tree, a browser must walk the entire DOM tree, and check which CSS rules apply to each node. The more unused CSS there is, the more time that a browser might potentially need to spend calculating the styles for each node.

Unused CSS

1.  Text Compression Encoding

Enable text compression on your server.

content-encoding

To compare the compressed and de-compressed sizes of a response:

Enable large request rows: size column — top value uncompressed. bottom compressed.

undefined

4\. Estimated input latency

Record run time performance & Record load time performance

5\. First contentful paint

minimize javascript, content-encoding, tree shaking, code splitting, http caching

6\. First CPU Idle

  

Serve webp images

Make sure server provides ETag for http caching

Minify css using webpack to reduce bytesize

IntersectionObserver for offscreen image loading

  

PROGRESSIVE WEB APPS

1.  theme-color meta tag and in manifest.json
