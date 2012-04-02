---
layout: post
title: "在 Redmine 中加入 Git Repository"
date: 2012-04-02 12:36
comments: true
categories: [Redmine,Git,gitolite]
---

相信 [Redmine](http://www.redmine.org/) 是很多人在使用的專案管理系統，而 [Git](http://git-scm.com/)，相信也是各種 SCM 裡的大宗，而應該滿多人使用 [gitolite](https://github.com/sitaramc/gitolite) 來控管自己的 git remote server 的。

之所以寫這篇，是因為小弟公司在整合兩者的時候，撞到了 gitolite 的 repo 權限問題。

所以這篇就來簡單的講解一下如何在 Redmine 裡面加入使用 gitolite 管理的 Repository ， 可在 Redmine 直接觀看 Source Repo，以及作為 Code Review 之用。

<!-- more -->

首先我們要了解的是，**Redmine 僅能透過檔案系統來存取本地端的 Git Repo**，它並沒有辦法透過 Git protocol 來存取遠端的 Repository。 但是就算是 Local 端的 Repo ， 在 gitolite 的管理之下，Redmine 還是沒有權限去存取 gitolite 控制的 Repository。

因此首先我們要製作一個 Redmine 可以存取的鏡像 Repo。從 Git 1.6 開始我們可以直接從一個 bare repository clone 產出另一個 bare repository。

首先建立一個資料夾來存放我們待會要 clone 的 repository。

*這邊開始以 `/var/git-miroors` 資料夾為例子：

```bash
$ mkdir /var/git-mirrors
$ cd /var/git-mirrors/
```

再來我們要使用 `git clone --mirror` 建立一個原始 repo 的鏡像：

```bash Folder : /var/git-mirrors/
$ git clone --mirror your/gitolite/path/your-repo.git
```

接下來下一步，由於之後 `git` 使用者會透過 `post-receive` hook 來同步兩個 repository，我們直接修改這個境像 repository 的 owner / group 為 `git` 來讓它有讀寫權限。

```bash Folder : /var/git-mirror
$ chown -R git:git your-repo.git
```

再來我們要設定原本 repo 的 [hooks](http://sitaramc.github.com/gitolite/hooks.html) ：

```bash
$ cd your/gitolite/path/your-repo.git
$ vi post-receive
```

在 hook `post-receive` 設定 git push 到鏡像 repo 的動作：


```sh post-receive
#!/bin/sh
/usr/bin/git push --mirror /var/git-mirrors/your-repo.git
```

建立完 `post-receive` 檔案後修改權限：

```bash
$ chown git:git post-receive
$ chmod 700 post-receive
```

由於 Git 在做 mirror push 的時候，會保留原始的檔案與資料夾存取權限，在 gitolite 控管下，只有 owner 有讀寫權限，所以一做 mirror push 這個鏡像的 repository 就沒辦法被其他 process 讀取到了，所以我們一開始就要告訴這個 mirror repository 它是被分享的，並且設定它應該要有的存取權限：

```bash
$ cd /var/git-mirrors/your-repo.git
$ chmod a+rX -R ./
$ git config --add core.sharedRepository 644
```

然後在 Redmine 的 Project / Settings / Repository 裡，設定路徑：

![](http://f.cl.ly/items/0v2V1k0i1y323G2k3S45/Screen%20Shot%202012-04-02%20at%203.03.20%20PM.PNG)

點選 Project / Repository 等待一下，讓 Redmine 處理 Repository 的資訊，然後看到 Repo 的資訊：

![](http://f.cl.ly/items/0P2f0F3K1q402L3Q3a0I/Screen%20Shot%202012-04-02%20at%203.04.53%20PM.PNG)

收工 :D



