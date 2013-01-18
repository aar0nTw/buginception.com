---
layout: post
title: "Javascript 中如何正確判斷類型"
date: 2013-01-18 23:29
comments: true
categories: [javascript]
---
> JavaScript 中如何正確的判斷類型，這東西也許基本，但其實隱藏了許多細節，雖然已經 2013 年了，在 JavaScript 中判斷正確類型依然是個悲劇 (無誤)

## typeof #

先講講 typeof

```js typeof
typeof "foo"          // string
typeof 123            // number
typeof (function(){}) // function
```
上面看起來挺正常的

```js
typeof []    // object
typeof null  // object !!(?)
```
