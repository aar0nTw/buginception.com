---
layout: post
title: "用Dropbox來當做Git remote"
date: 2012-02-22 01:54
comments: true
categories: [git,dropbox]
---

這篇筆記兼分享一下如何使用[Dropbox](www.dropbox.com)簡單建立git remote進行多人協同開發。

當然不是單純的把專案開在Dropbox folder而已，那Source可是會亂掉的:P

<!-- more -->

首先我們在Dropbox建立一個資料夾專門放我們的Project

```bash
~$ cd Dropbox
~/Dropbox$ mkdir project
```

接著建立一個放置repo的資料夾，並且建立git repo

```bash
~/Dropbox$ cd project
~/Dropbox/project$ mkdir myproj.git
~/Dropbox/project$ cd myproj.git
~/Dropbox/project/myproj.git$ git init --bare
```
這邊用了`git init --bare`這個指令，它並不會產生`.git`資料夾，而是會把`.git`裡面的內容直接產生在目錄下面，並且在往後commit/push到這裡的更動，是不存下source code的，你需要將更動**拉回家**才會有所動作，但是這裡仍然記錄了所有的變動，`git init --bare`很適合拿來節省空間。

而且使用Dropbox來做remote你也不希望電腦裡有兩份一樣的code在不同的資料夾下吧:)。

接著，我們到本機的repo底下用Dropbox的路徑設定一個新的remote，並且push master branch到**dropbox** remote

```bash
~/myproj$ git remote add dropbox ~/Dropbox/project/myproj.git
~/myproj$ git push dropbox master
```
接著把Dropbox裡的repo分享給要co-work的人，對方用同樣的方式在設定一次remote，大功告成:)

