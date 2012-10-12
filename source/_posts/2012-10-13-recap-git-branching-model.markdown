---
layout: post
title: "成功的 Git 分支模式"
date: 2012-10-13 00:27
comments: true
categories: [git,cocoaheads] 
---
![image](/images/137c1365ae01d749bcfbf502f51c9f0d.jpeg)

(image via [Bartek Elsner](http://www.behance.net/gallery/The-Branch/3680339))

>這週四，有幸參與了 CocoaHeads Taipei 十月份的聚會，並且跟與會的朋友們分享了公司內部管理 Git 分支的模式，其實這個模式其實非常流行了，最早是由 [Vincent Driessen](http://nvie.com/posts/a-successful-git-branching-model/) 先生於他的 Blog 分享的，並且有許多團隊都是採用這麼一套 flow，這篇文章主要是整理 CocoaHeads 當天我所分享的內容，並且做更詳細的補充。
<!-- more -->

#Why Git ?

先談談為什麼要用 Git，其實現在用 Git 是普世價值(?)，好像不用 Git 就弱掉了，當然不是，之所以用 Git 絕對有它的理由，可以以極低成本的開啟一個 Branch，絕對是 Git 吸引人的最大理由之一。

這邊讓我們先來談談 VCS（版本控制系統）的進化之路，看看從古至今，我們運用 VCS 的工作流程發生了什麼樣的改變：

## Lock

早期的一些 VCS，在多人開發模式的處理上，運用了一種極為簡陋與現今難以想像的方式，我們稱之為 **Lock**，每當其中一個開發者開始編修一個檔案的時候，便將該檔案鎖住，使其他的開發者無法同時編輯該檔案，也就是說當你有一個問題要解決，這個問題牽扯到其他檔案，如果其他檔案有人正在修改，那你可以說是寸步難行，這是一種過時而且非常差的處理方式。（就像我們難以想像現今仍然有公司不使用 VCS，只運用 email 與檔案壓縮工具互相交換 source code 一樣，那是一種口頭上的 Lock）

## Merge

後來，有些東西改變了，較為進步的版本控制系統，開始提供一個稱之為 **Merge** 的功能，開發者們開始可以同時對一個檔案進行更動，並且提交他的更改（**Commit**），如果發現有衝突（**Conflct**），開發者們可以互相的去修復衝突，藉此達到共同開發的效果。

## Pull

但是這樣其實是不夠的，僅僅只是 **Merge** 的功能，我們無法確保團隊之中所有人的修改都不會對自身的程式造成影響。那麼在現今更為先進的版本控制系統，例如 Git 之類的 DVCS（分散式版本控制系統）我們獲得一種新的模式稱之為：**Pull**，如果說 **Merge** 是人類發現了火，那 **Pull** 可以說是協同開發的工業革命，在分散式版本控管系統的特性下，一個團隊的 Leader，或說是一個 Project 的擁有者、主要維護者，可以接受由別人所發送的 **Pull Request**，將其他開發者所修改、新增的項目 **Pull** 下來，並且作 review，沒有問題才將之 **Merge** 到主要的 Repo，當然，這樣的方式，有很大負擔會落在主要的維護者身上，整體效率取決於 Project Owner 的效率，如果是一個龐大的商業專案，我們需要更有效率的方式。

## Review & Accept

那麼如果我們可以做到系統自動化測試、CI，然後每一次的 Pull Request，通過測試之後，整體團隊的任何人都可以來 Review、討論、最後接受這一次的修改，Project Owner 的效率問題所帶來的瓶頸，將不再是問題，對於整體 Project 的程式碼品質，以及團隊成員對每一部分的熟悉度，以及持續改善程式碼品質方面，都可以帶來莫大的幫助，我們無形中達成了許多團隊抽不出時間做到的，我們也許就不再需要花額外的時間來實行 Code Review。現今，Google Android Project 所使用的 [Gerrit](http://code.google.com/p/gerrit/) 系統，便是為了達成此種目的所開發的系統，而 GitHub 的 Pull Request 功能，同樣也可以達到相同的效果。

#Git Flow

當我們進入到了 Review & Accept 的階段後，我們更需要一個結構完整、清楚並且高效率的分支控管模式，這就是今天最重要的主題了。我將會一一介紹在這個模式中會出現的五個 Branch，下面我將從最重要的 Branch 開始。

## The Main Branches

* Master Branch
* Develop Branch

### Master Branch

第一個是 Git 使用者相當熟悉的 `master`，作為主要的分支，在這樣的模式之下，`master` 並不會是最新的開發內容，但 `master` 會完全的與正式版的內容相同，`master` 的內容就會是 Production 的內容。

### Develop Branch

而 `develop` 則會是最重要的分支，它會是所有開發里程碑的紀錄，而 Project 管理者在決定下一個正式版發布內容時也將完全運用 `develop` 來評估團隊下一個版本將 release 的內容。

而這兩個主要 Branch 將會是 Project 的骨幹，在 Project 的生命軌跡之中，只有這兩個 Branch，是永遠存在的。

## Support Branches

* Feature Branch
* Release Branch
* Hotfix Branch

這三類分支是我們口中的輔助，輔助 Branch 會在達成他的任務後便關閉，並 **merge** 回 main branch 之中。

### Feature Branch

當一個開發者準備開始在一個 Project 之中展開一個新功能的開發，開發者將會從 `develop` 轉移到所謂的 `feature` 之中，在這邊，我們團隊的慣例是以 `aaron-feature/` 這樣的 Prefix 命名慣例來開展一個新的 feature branch，例如：`aaron-feature/new-function` 類似這樣的 feature branch 命名方式。

而當我們完成一個 feature 的開發，我們將會把 `aaron-feature/new-function` **push** 到 GitHub  上，並且發送一個 **Pull Request**，進入 **Review** 的階段，團隊中的成員將會就這些功能與原始碼，展開討論與 Code Review 並給予建議與改善，最後認可，由團隊的其他成員將 feature branch **merge** 回 `develop` 並結束這一個 feature 的流程。

### Release Branch

`develop` 上完成的新 feature 越來越多，Project Owner 決定 Release 一個新的版本，此時我們將會展開一個新的 Release Branch 如 `release-v2.0.0`，開始進入 **Regression test** 的階段，此時最重要的原則便是，進入 `release` 分支之後，只有在 `release` 修正問題後所產生的 commit 可以合併回 `develop`，而 `develop` 中任何的新改變，都不可 **merge** 回 `release` 否則變失去了做 Release Branch 的意義。

在完成了 Regression test 之後，我們便可以發佈一個新的正式版了，這時候我們將會把 `release` 分支中所作的改動合併回 `master` 與 `develop` 並關閉這一個 release branch，並且在 `master` 之中加上新的 Tag `v2.0.0` 完成這一次新版本的 Release。

### Hotfix Branch

百密一疏，當我們發現在 Production 的環境之中，有在 Release 階段未能發現的嚴重 Bug 需要即時的做修復，我們便會進入到所謂 **Hotfix** 的階段。其實某種程度上來說，`hotfix` 與 `release` 是非常相似的，它們最終的目的都是在 `master` 上發佈一個新版本，只是緣由不同而已。

例如我們某天在正式版上發現了一個嚴重的 Bug 已經影響整個程式的運行，我們將會從 `master` 展開一個名為 `hotfix-v2.0.1` 的 branch，當我們完成 bug 的修復，同時將 `hotfix` 的內容 **merge** 回 `develop` 與 `master` 並發布一個新版本 `v2.0.1`

# No Fast Foward

```bash
$ git merge --no-ff
```

也許有些人喜歡使用 Fast Foward 來讓 Git 的線圖保持乾淨，但其實使用 Fast-Foward 並沒有帶來任何好處，不使用 fast-foward 是很重要的，也許使用 `no-ff` 會使 log 上多出一個 commit，但它將使團隊的開發過程有詳細的脈絡可循，所帶來的好處遠比少一個 commit 來的重要與龐大。


# Tools

如果想要快速的開始運用這樣的模式與流程，Vincent Driessen 也開發了一個 git plugin：[git-flow](https://github.com/nvie/gitflow)，讓開發者可以很輕易的使用這樣的分支模式。

```bash
$ brew install git-flow
$ mkdir your-project
$ cd your-project
$ git flow init
```

# Slide




<p>
<center><iframe src="http://www.slideshare.net/slideshow/embed_code/14685015" width="597" height="486" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC;border-width:1px 1px 0;margin-bottom:5px" allowfullscreen> </iframe></center>
</p>