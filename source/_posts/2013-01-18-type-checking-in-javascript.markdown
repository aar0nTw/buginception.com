---
layout: post
title: "淺談類型判斷 in JavaScript"
date: 2013-01-30 12:52
comments: true
categories: [javascript]
---
![image](http://cdn.buginception.com/public/images/7147080735_95308bc455_b.jpeg)

( image via [Franco Folini](http://www.flickr.com/photos/livenature/7147080735/) )

> JavaScript 中如何正確的判斷類型，這東西也許基本，但其實隱藏了許多細節，雖然已經 2013 年了，但是這個看似簡單的問題在 JavaScript 中也許不會有正確且單一的答案。

在 JavaScript 中判斷類型，有非常多種選擇，但要記住，絕對不要只選用一種方式來判斷類型，因為不管是哪種選擇，都有其特異的行為與例外，適當的選擇變數的判斷方式，在 JavaScript 的寫作中是非常重要的。

<!-- more -->

## typeof #

其實光是 `typeof` 這個方法就可以花整整一個篇幅來講，有太多的事蹟可以說嘴了：

```javascript 下面是一段荒唐的原始碼
// 有時候你會看到這樣的 Code
if (typeof foo !== 'undefined') {
  /* 這裡有些可笑的 Code … */
}
// 或是
if (typeof foo === 'object') {
  /* 這邊寫些詭異的事情 … */
}
```

上面是段常見的 JavaScript code，運用 `typeof` 來判斷變數的型態，但當你用 `typeof` 習慣之後，你會發現有趣的事：

```js 
typeof "foo"          // "string"
typeof 123            // "number"
typeof function(){}   // "function"
typeof undefined      // "undefined"
typeof {}             // "object"
typeof false          // "boolean"
```

It's work！以上這些情況，看來都沒有什麼問題，但是：

```js
typeof []    // "object"
```

一個 array 是 `object`，這並非錯誤的結果，但這表示我們必須要另外的條件去判斷 `array`，不過如果是下面這樣呢：

```js
typeof null // "object" (It's a old mistake ….)
```

> 部分關於 `typeof null` 的討論，有興趣可以看看 [proposals](http://wiki.ecmascript.org/doku.php?id=proposals:typeof) 跟 [discussion](http://wiki.ecmascript.org/doku.php?id=discussion:typeof) 

不過 V8 engine 倒是有提供選項修正這件事，可以試試看使用 `--harmony_typeof` 這個選項

```sh
$ node --harmoy_typeof
> typeof null
'null'
>
```
`--harmony_typeof` 在 V8 中是一個獨立的 option 而不是被整併在 `--harmony`，這部份可以看看 [harmony:typeof_null](http://wiki.ecmascript.org/doku.php?id=harmony:typeof_null) 的討論；就像 [Douglas Crockford](https://plus.google.com/118095276221607585885/posts) 講到的：

>**I like this fix. New code will definitely be better for it. But the old code breaks.**

這樣的修正由於影響了太多舊有的程式，因此在 Harmony 中是被 reject 了。

看來大家應該了解到了，在 JavaScript 中，單單運用 `typeof` 是不足的，看起來甚至有點滑稽，日子一過，時間一久，是人都會撞到一兩個雷，所以你最好離他遠遠的。

```js 另外一提
typeof NaN // "number" Not a Number is a Number type
```
> 為什麼 `typeof NaN` 是 `number` 呢？這是因為 ECMAScript 標準規定 Number 必須是 IEEE-754 浮點數，這之中包含了兩個特殊數值：`Infinity`, `NaN`

## instanceof

那麼 `instanceof` 這個方法呢？用以判斷物件本身繼承何種類型，但這個操作符有其運作的限制，底下我們來看個例子：

```js
{} instanceof Object               // true
[] instanceof Array                // true
/abc/ instanceof RegExp            // true
(function(){}) instanceof Function // true
```
上面跟 `Object` 有關的情況下運作都非常正常，但下面的狀況就不一樣了：

```js
1 instanceof Number         // false
false instanceof Boolean    // false
'string' instance of String // false
NaN instanceof Number       // false
```
這邊要提到的是，JavaScript 中的基本型別，是無法以 `instanceof` 來判別的。

<blockquote class="note">
<p>
基本型別：string、number、boolean
</p>
</blockquote>

但可以這樣寫

```js
new Number(1) instanceof Number  		// true
new String("str") instanceof Number	// true
```
不過這樣的程式沒什麼意義，因為你早就知道他是什麼型別的了 :)

### constructor

或是我們可以使用 `constructor` 屬性來判斷

```js
(1).constructor === Number      // true
NaN.constructor === Number      // true
false.constructor === Boolean   // true
'string'.constructor === String // true
```
不過 `constructor` 也有運用上的難題，那就是無法合理的判斷繼承的類型：

```js inherit example
function Car (){}
function Ford () {}
Ford.prototype = new Car
Ford.constructor // Car
```
而現今大部分的 js lib (or framework) 都有自己實作繼承的方法，可謂其濫觴。

<blockquote class="note">
<p>
你會需要這本書：<a target="_blank" href="http://shop.oreilly.com/product/9780596806767.do">JavaScript Patterns, Stoyan Stefanov, 2010.</a>
<p>
</blockquote>

另外如果是跟瀏覽器打交道的前端工程師，在這部份需要的知道的是，不同 Window 之間的物件體系是各自獨立的：

```js 這邊有一段你平常不會寫的 code
g = open("http://www.google.com.tw")
a = new g.Array(1,2,3) // [1, 2, 3]
a instanceof Array     // false
```
你的 `Array` 可不是我的 `Array` 啊！

## Object.prototype.toString

前面我們提到了可以運用 `constructor` 屬性來判定物件類型，讓我們再來講講 `Object.protype.toString` 這個方法

```js
Object.prototype.toString.apply({}) // "[object Object]"
Object.prototype.toString.apply([]) // "[object Array]"
Object.prototype.toString.apply(NaN)// "[object Number]"
Object.prototype.toString.apply(function(){}) // "[object Function]"
```
運用這種方式我們可以正確的判斷一個變數的基本型態，但是如果是自訂類型的話，卻無法得知真正的類型，因為結果依然會是 `[object Object]`

## 小結

在 JavaScript 中要正確判斷類型，當仔細去鑽研的時候，真是一件麻煩事，根據不同的情境去設計你的判斷式是相當重要的，我們也必須要去思考如何用最簡潔的方式判斷正確的類型，當然這篇還有很多地方沒有介紹到，例如 `isPrototypeOf` 這個方法，JavaScript 是一個有許多歷史包袱的語言，但也是不斷的在進步，運用它的時候，要注意，有太多的方式是雙面刃，切記要小心運用。

<blockquote class="note">
<p>
你會需要這本書：<a target="_blank" href="http://oreilly.com/catalog/9780596517748/">JavaScript: The Good Parts, Douglas Crockford, 2008.</a>
<p>
</blockquote>

## 其他

不少 framework 也有實作很多類型判斷的方法，下面是一例
