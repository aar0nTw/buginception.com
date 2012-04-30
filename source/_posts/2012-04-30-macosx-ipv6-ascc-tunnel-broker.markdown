---
layout: post
title: "Mac OSX 下使用中研院的 tunnel broker"
date: 2012-04-30 22:28
comments: true
categories: [ascc,tunnel broker,ipv6,gw6c]
---

> 今天 [[問卦] 中華電信HiNet連YouTube慢的真相?](http://disp.cc/b/570-3ymq) 這件事好像很夯，包含之前 Mobile01 上面也有相關文章：
> [中華電信HiNet連YouTube慢的真相-人為互連障礙](http://www.mobile01.com/topicdetail.php?f=507&t=2612039&p=1)，
>我參考了下面這篇文章稍作修正讓 osx 也能用中研院的 tunnel broker：
>[Gentoo Gateway 連中研院的 IPv6 Tunnel Broker](http://blog.richliu.com/2012/04/28/1204/) via [richliu's blog](http://blog.richliu.com)

<!-- more -->

環境需求
-------
1. 首先，你必須是 Mac OS X （嗯對，這篇是教你在 OS X 上用，其他環境的教學已經有了）
2. 你要有 `root` 權限
3. 你要有開發環境，因為我們需要 C compiler，裝個 [XCode](https://developer.apple.com/technologies/tools/) 吧！

先到 [http://tb2.ipv6.ascc.net](http://tb2.ipv6.ascc.net/) 申請一個帳號，我是選第一個，亂數產生密碼寄到您的信箱。

TunTap Driver
-------------
再來，你需要安裝一下 [TunTap Driver](http://tuntaposx.sourceforge.net/)，這是幹嘛的呢？他讓你的 OS X 可以新增一個虛擬的網路介面，到網站上下載 .pkg 檔安裝就可以了。最新的 release 版本是 [tuntap_20111101](http://downloads.sourceforge.net/tuntaposx/tuntap_20111101.tar.gz)，如果不安裝的話，等下 gw6c 是跑不起來的。

Gateway 6 client
----------------
再來請到 [go6.net](http://www.go6.net/4105/download.asp) 下載 gw6c 的 source，這邊我們要自己 compile client 端，選擇 **Client 6.0 Source Code (Linux/Unix/Darwin/BSD)**，解壓縮。

因為我也滿懶，所以我就直接在下載資料夾下面搞了：

```bash
$ cd ~/Downloads/gw6c-6_0_1/
$ cd tspc-advanced
$ chmod +x template/*.sh
$ make all
```
會在 `./tspc-advanced/bin` 下產生 gw6c 的執行檔

其實你也可以做 install：

```bash gw6c-6_0_1
$ cd tspc-advanced
$ make clean
$ make platform=darwin installdir=/usr/local/gw6c install
```
建議是裝在 `usr/local/` 下面
然後改寫一下設定檔，拿內附的範例設定來改：

```bash usr/local/gw6c/
$ cd bin
$ vim ./gw6c.conf
```
編輯 `gw6c.conf`，大致上就是：

```bash gw6c.conf
userid=剛剛申請的帳號
passwd=剛剛拿到的密碼
server=tb2.ipv6.ascc.net
auth_method=digest-md5
host_type=host
if_prefix=en0
```
啟動吧！：

```bash ./tspc-advanced/bin
//這是沒 install 的情況
$ pwd
/Users/apple/Downloads/gw6c-6_0_1/tspc-advanced/bin
$ sudo ./gw6c -f gw6c.conf
```


```bash usr/local/gw6c
//有 install 就直接來吧
$ pwd
/usr/local/gw6c/bin
$ sudo ./gw6c
```

<blockquote class="note">
<p>
如果你的是 gogoc，步驟也大同小異，應該說一樣 :P，這裡有 gogoc 的 <a href="http://gogo6.com/downloads/gogoc-1_2-RELEASE.tar.gz">source</a>
</p>
</blockquote>

```bash
$ ifconfig
```
`tun0` 有起來就是成功了

看一下 ipv6 的 google 吧！[ipv6.google.com](http://ipv6.google.com)

後記
---
其實好像也沒變多快，不過看 full HD 的有差就是了。


