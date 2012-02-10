---
layout: post
title: "[Yii]Ch.0-3 Yii環境建置"
date: 2012-02-09 16:52
comments: true
categories: [php,yin,tutorial]
---
這一篇可能是一個比較短的篇幅，來講環境建置的部份，上一篇已經有稍微的講到了環境需求，那我也不在重複一次，接著請下載最新的YiiFramework

>[Yii 官方網站下載](http://www.yiiframework.com/download/)

或者是github上熱心人士非官方的source，目前更新到1.1.9

```bash
$ git clone https://github.com/dmtrs/unofficial-yii-framework-mirror.git 
```

解壓縮到網站根目錄下，取個簡單的名字，例如：yii

```bash
$ tar -xzf yii-1.1.9.r3527.tar.gz
$ mv ./yii-1.1.9.r3527 yii
```

接下來我們打開Yii requirement頁面來看一下yii系統需求檢查：

<!-- more -->

```
http://localhost/yii/requirements/
```

可以看到類似下面的畫面：

![Yii系統需求檢查](/images/article/ch0_3_01.PNG)

如果你是一個新建立的網站環境，可能會有大部分的php extension是未通過的狀況，
上圖中，由於我這邊沒有memcache server所以除了memcache extension之外其餘都是通過的狀態，這邊建議是使用者是YiiFramework的extension是必備的，其餘元件看個人需求安裝。

* [APC](http://pecl.php.net/package/APC)
* [mcrypt](http://www.php.net/manual/en/mcrypt.installation.php)
* [Memcache](http://www.php.net/manual/en/memcache.installation.php)
* [GD](http://www.php.net/manual/en/image.installation.php)

當我們測試完Yii Requirements，可以打開yii demo頁來看看：

```
http://localhost/yii/demos/blog
```

可以看到一個採用YiiFramework的Blog：

![Yii Blog Demo](/images/article/ch0_3_02.PNG)

到這邊為止，可能會遇到Demo頁面遇到以下狀況：
```
Application runtime path "/var/www/html/yii/demos/blog/protected/runtime" is not valid. Please make sure it is a directory writable by the Web server process.
```

或者是

```
CAssetManager.basePath "/var/www/html/yii/demos/blog/assets" is invalid. Please make sure the directory exists and is writable by the Web server process.
```

這時只要修改一下**assets**跟**protected/runtime**的資料夾權限讓php可以存取，就沒有問題了:)