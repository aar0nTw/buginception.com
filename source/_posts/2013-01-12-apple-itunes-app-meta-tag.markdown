---
layout: post
title: 'iOS6 中的 apple-itunes-app tag'
date: 2013-01-12 16:05
comments: true
categories: [mobile safari,ios]
---
![image](http://cdn.buginception.com/public/images/tumblr-iphone-app2.png)

> 上週某一天，我在逛 tumblr 的時候，發現了一個有趣的 meta tag，Jamie Sa ( [@jamex](http://twitter.com/jamex) ) 他逼我一定要寫一篇文講這個。{% emoji scream %}

當我們運用 iPhone 逛到某些網站的時候，會看到他的最上方出現了一個 itunes 的 Banner，像是下面這樣：

<!-- more -->

![tumblr](http://cdn.buginception.com/public/images/tumblr-iphone-app.png)

其實要知道 Device 上有沒有自己的 app 在以往的作法並不難，只要 App 去攔截一個 Custom URL schema，然後網頁發送一個 Request，例如 `tumblr://`，JavaScript 這邊去擷取成功跟失敗的事件即可。

但是，當我發現我刪除該服務的 app 時，這個 Banner 變成了下面這個樣子：

![image](http://cdn.buginception.com/public/images/tumblr-iphone-app2.png)

!!! (狀態顯示為大驚 {% emoji astonished %}{% emoji astonished %}{% emoji astonished %})

這肯定不是以前那種做法，這個時候新版 safari 的 device inspector 就派上用場了，讓我來看看這是什麼巫術 {% emoji smirk %}

![image](http://cdn.buginception.com/public/images/app-itunes-meta.png)

根據 [apple 的文件](http://developer.apple.com/library/ios/#documentation/AppleApplications/Reference/SafariWebContent/PromotingAppswithAppBanners/PromotingAppswithAppBanners.html)指出，這是 iOS6 之後 safari 所支援的新功能，你不用再用自製的方式在你的網站上彈出一個 Banner 宣傳你的 iOS app 了，你可以運用這個新的 meta tag，讓 iOS 由一個統一的介面來幫你完成這件事。

```html apple-itunes-app
<meta
  name="apple-itunes-app"
  content="
    app-id=myAppStoreID, 
    affiliate-data=myAffiliateData, 
    app-argument=myURL"
    >
``` 

`app-argument` 是一個選擇性的欄位，你可以選擇攔截一個特殊的 URL schema 並透過這個參數讓使用者能夠轉到 app 上對應的頁面。

下面是 tumblr 的寫法：

```html apple-itunes-app
<meta 
  name="apple-itunes-app" 
  content="
    app-id=305343404,
    app-argument=tumblr://x-callback-url/dashboard?referrer=smart-app-banner"
>
```

國內其實有些服務已經加上了這個 tag，還沒有加入這個 tag 或是還在使用舊方法的朋友們，趕快在你的 web 頁面上加入這個 tag 吧 {% emoji grin %}
