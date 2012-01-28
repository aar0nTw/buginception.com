---
layout: post
title: "[Yii]Ch.0-2 事前準備"
date: 2012-01-28 16:21
comments: true
categories: [yii,php,tutorial] 
---

上篇我提到了在PHP裡採用Yii框架有什麼好處，那麼這次開始就要來提到如何採用Yii，需要哪些事前準備.

用Yii來開發Web Application，你必須要有PHP的基礎這是一定的，在之後我所講的内容基本上都是預設讀者有基本的PHP知識，另外還需要有PHP OOP的相關知識或者實戰經驗。

<!-- more -->

進入正題，我們開發yii需要什麽事前準備呢:

### 1. 開發環境
開發環境，重點當然是一臺電腦，不管是Windows，OSX，Linux的作業系統，用的順手就好，我個人是用2010年的MBA來做爲我開發的平臺，另外還用虛擬機安裝了一台Ubuntu Server 11.10，作爲測試環境.

PHP runtime的版本是5.3.5，這邊建議PHP的版本最好是5.3以上，因爲5.3跟5.2在OOP的GC機制上有做了重大的改變，效率更好，memory leak的情形大幅减少.
### 2. 編輯器
我採用過的編輯器很多，編輯器也是用的順手就好，好的工具可以讓開發的時間縮短，開發起來更方便，更易於找尋錯誤，但是並沒有哪個比較好的問題，高手用記事本也還是高手。

這邊推薦幾個編輯器:

1. [Aptana Studio](http://aptana.com)
>從eclipse衍申而來的老牌IDE，跨平台，我學生時期一直到剛進公司都是採用Aptana在做開發，優點是跨平台，eclipse plugin眾多，本身支援JavaScript的開發，有多種JavaScript Framework如jQuery，Prototype，extJS的自動提示功能，缺點就是太肥大了，OSX上使用有時候感覺不是很順，還會發呆:(
2. [TextMate](http://macromates.com/)
>熟悉Ruby開發的開發者不可能不知道的一個編輯器，也是老牌子了，不過中文支援有很大的問題。付費軟體
3. [Sublime text 2](http://www.sublimetext.com/2)
>我現在使用的編輯器之一，介面漂亮，跨平台，Plugin也不少，而且免費使用，中文也沒有問題，而且除了PHP，其他語言的支援性也很好，尤其是Python，沒事會寫寫Python的玩家們，我很推薦這一套編輯器。
4. [vim](http://www.vim.org/)
>常操作unix系的OS必學的編輯器，有很多的plugin，當然terminal下只有全鍵盤的操作方式，但是習慣了實在是會上癮，我連sublime text都有加入vim的操作快捷，可見對其操作中毒之深，我常常會有機會直接ssh到測試server上修改一些小東西，vi/vim的操作是必學的。

### 3. 版本控制
我傾向於一開始學習程式設計，就要跟著學習版本控制系統的使用，目前我個人使用都是用Git來做為我的版本控制系統，Mac本身就內建了Git，很是方便，而公司則是SVN/Git兩種系統來管理的專案皆有，使用版本控制系統能夠有效的來管理專案，尤其是多人開發的時候，有效的來掌控原始碼，甚至在準備deploy專案的時候也派的上用場。

如果要採用Git，可以先看看這篇 [Git - the simple guide.](http://rogerdudler.github.com/git-guide/) 簡單的了解到Git是怎麼樣的一個系統，另外，也去申請一個 [GitHub](http://www.github.com) 的帳號，除了免費的remote server之外，也可以從這個龐大的PG社群獲得許多的技術寶物。

>工欲善其事，必先利其器

有了好的開發環境與工具，可以讓專案的開發事半功倍，加速開發的時間與效率，但是有了方便的開發工具，也別忘了打好基礎，雄厚的語言基礎與知識才是最強大的後盾與武器。