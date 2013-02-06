---
layout: post
title: "Git 指令調味"
date: 2013-02-06 23:17
comments: true
categories: [git]
---

![red chile](http://cdn.buginception.com/public/images/477bbb34c40ed50f2013c93702c588b6.png)

( Image by [Nancy McCabe](http://www.behance.net/gallery/Indian-as-Apple-Pie/6206911), CC license )

> 本篇介紹常用 git 指令在實際工作上會使用的呼叫方式，從 `commit`、`push`、`pull`、`add` 等指令添加 option，使其更貼近於日常使用需求。

<!-- more -->



```sh 1.diff
$ git diff -b HEAD --
```

查看尚未 commit 的更動與目前版本的差異

***加上 `-b` 或 `--ignore-space-change` 忽略空格的更動（例如縮排）***

```sh 2.diff-2
$ git diff HEAD HEAD^
```

查看當前版本與上一個版本之間的差異

*又 `HEAD~2` 是上兩個版本、`HEAD~3` 是上三個版本，類推。*

<blockquote class="note">
<p>
運用 git 的 alias 功能設定自己的指令別名，精簡過長的指令與 option。
</p>
</blockquote>

```sh 3.push
$ git push -u [remote] [branch]
```

push 到遠端。 ***加上 `-u` 或 `--set-upstream` 設定 upstream reference。***，設定 **upstream** 後，之後可以直接 pull 不指定 remote branch

```sh 4.push-2
$ git push [remote] :[branch]
``` 
刪除 remote 上的 branch

```sh 5.push-3
$ git push --prune
```

push 並同時刪除 remote 上不存在於 local 的 branch

```sh 6.fetch
$ git fetch --prune
```

刪除 local 上有 remote-tracking 但 remote 已經不存在的 branch

```sh 7.rebase
$ git rebase [branch]
```

rebase 使用情況： `develop` branch 上剛 apply 了一個 pull-request，但是我**已經在 local 上舊的 `develop` branch checkout 了一條新的 feature branch 上繼續往下動工了**，那當我們 local 拉下新的 develop 後，準備發送一個新的 merge pull-request 之前，***先在 feature branch 上做 `rebase develop` 的動作***，將新舊 `develop` 的差異補齊，可以**減少 merge conflic 的時間，並且用 rebase 的方式可以避免多餘的 merge 動作**。

```sh 8.checkout
$ git checkout -b [branch]
```

切換 branch，*加上 `-b` 如果 branch 不存在直接開啟新的 branch。*

```sh 9.describe
$ git describe --tags
```

顯示從最近的一個 tag 到目前版本經過了幾次 commit，這個指令我常用來確認兩個 branch 差了幾個 commit，***拿來快速產一個版號也是很方便。XD***
