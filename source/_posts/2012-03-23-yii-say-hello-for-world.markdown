---
layout: post
title: "[Yii]Ch.0-4 建立第一個Web app"
date: 2012-03-23 19:25
comments: true
categories: [yii,tutorial]
---

> 最近公事繁忙，一回神已經兩週沒更新blog了orz。
> 現在Yii framework，已經merge到github上了，可以clone下來對yii作版本管理:)

```bash
$ git clone https://github.com/yiisoft/yii.git
```


前面講到的部分，基本上是屬於環境配置的部分，接下來我們要正式的進入實作階段:)

首先建立一個新的Web App
（這邊以~/htdocs 為網站根目錄）

```bash
$ yiic webapp ~/htdocs/demo
```
<!-- more -->

yiic是YiiFramework內建的一個console工具可以快速建立web app，作migration，shell模式下可以建立model，scaffold，module等等，建議使用強大的網頁介面Gii，不過我還是習慣用console :D

建立了一個新的web app之後，首先我們可以看到資料夾內作為bootstrap的index.php，這邊稍微簡單講一下：

```php index.php
<?php

// change the following paths if necessary
$yii=dirname(__FILE__).'/../yii/framework/yii.php';
$config=dirname(__FILE__).'/protected/config/main.php';

// remove the following lines when in production mode
defined('YII_DEBUG') or define('YII_DEBUG',true);
// specify how many levels of call stack should be shown in each log message
defined('YII_TRACE_LEVEL') or define('YII_TRACE_LEVEL',3);

require_once($yii);
Yii::createWebApplication($config)->run();
```

可以看到require了yii.php（Production環境可以改用yiilite.php），定義了DEBUG跟TRACE_LEVEL。
這兩個常數會決定yii會不會寫入log，以及log與錯誤訊息顯示的詳細程度。

Production環境中為了提高app的performance這兩個常數會定義為false跟0。

然後我們可以看到Yii將會以$config變數指定的config檔案開啟一個yii app。

首先我們要了解一下Yii的檔案結構:

```bash demo/
drwxrwxrwx   3 aaron  staff  102 Mar 23 16:36 assets
drwxr-xr-x   8 aaron  staff  272 Mar 23 11:39 css
drwxr-xr-x   2 aaron  staff   68 Mar 23 11:39 images
-rw-r--r--   1 aaron  staff  497 Mar 23 11:41 index-test.php
-rw-r--r--   1 aaron  staff  574 Mar 25 03:35 index.php
drwxr-xr-x  18 aaron  staff  612 Mar 23 11:39 protected
drwxr-xr-x   3 aaron  staff  102 Mar 23 11:39 themes
```

assets的部分，其實就是運用`CAssetsManager`類別來控管或自動產出包括js,css等有可能造成衝突的檔案，有機會在詳細的討論，css,images，可以不透過Yii直接引用的靜態css,image檔案，theme則是不使用預設的them template時，放上自己或是別人寫的theme。 protected資料夾則是整個yii app的靈魂所在，這邊我們就另外的來看一下。

```bash demo/protected/
   -protected
   |---commands
   |-----shell
   |---components
   |---config
   |---controllers
   |---data
   |---extensions
   |---messages
   |---migrations
   |---models
   |---runtime
   |---tests
   |-----fixtures
   |-----functional
   |-----report
   |-----unit
   |---views
   |-----layouts
   |-----site
   |-------pages
```

protected資料夾簡單分成幾個部分：

* components
* config
* controllers
* extensions
* models
* tests
* views

**components**放置一些可以重用的開發者自行定義的元件
**config**是全站的設定檔，包含網站與cosole app還有測試環境的設定

**controllers**,**models**,**views**三個資料夾是放置mvc結構的檔案，**controllers**裡面的class name跟views裡的檔案結構是有相對應的關係的，預設裡面有一個**SiteController**，**views**裡面會有相對應的**site**資料夾存放他的前端呈現的頁面。**models**則是放置了所有資料庫相關的**ActiveRecord** Class。

**tests**資料夾則是放置PHPUnit的測試scripts，使用gii或是yiic shell產生Model時，會自動的產生出相對應的ModelTest類別，對有做單元測試的開發者來講可以相當方便的使用PHPUnit來作測試，也可以使用Selenium來作。

Yii整體的優點其實很多，就這篇的資料夾結構來講可以看出，其Yii已經幫你產出了測試框架，在做UnitTest,Functional Test可以很快的進入狀況，尤其實你是屬於採用TDD方法來開發的開發者。:)
