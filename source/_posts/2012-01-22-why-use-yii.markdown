---
layout: post
title: "[Yii]Ch.0-1 YiiFramework簡介 "
date: 2012-01-22 19:55
comments: true
categories: [yii,php,tutorial]
---
PHP的framework非常的多樣,隨手拈來都可以講出很多,舉例如老牌的[CakePHP](http://cakephp.org/), [Zend](http://framework.zend.com), [Symfony](http://www.symfony-project.org/), [CodeIgniter](http://codeigniter.com),或者是[SlimFramework](http://www.slimframework.com),抑或者最近常聽到的[fuelPHP](http://fuelphp.com),到我所要提到的[Yii](http://www.yiiframewok.com).

Yii,為什麼要採用Yii? 這邊有幾點來稍微簡介一下:

###1. High Performance
官網比較了同樣未採用APC與使用APC之後的RPS數據,Yii 在未使用APC的狀況下,RPS大約比CI多約15%,Cake的3倍,但在使用了APC之後Yii的RPS是CI的3倍,Cake的8倍,Symfony的13倍

至於Yii為何能做到?
<!-- more -->

主要的原因應該在於整個Yii Framework大量採用了lazy loading的技巧,舉例來說,在一個class file未被使用之前,這個檔案不會被include,在一個物件將要被使用之前Yii也不會初始化該物件,其他框架的缺點在於縱使未被使用或沒有被Request的功能例如DB connection,user session等,都還是被framework本身所啟用。

###2. MVC pattern
Yii相當完整的實作了MVC模式,MVC模式是近年來相當熱門的設計模式,概念上相當合理分離了商業邏輯與流程管控,但是實務上的MVC其實常常容易走調,這我之後有機會會在提到,簡而言之,在第一線講究使用者體驗的軟體實作上(包含了web,mobile app)MVC模式是相當熱門的模式,舉凡RoR,Spring,ASP.net,皆有MVC模式的實作. 當然許多的PHP framework也實作了MVC模式,Yii的MVC實作基本上參考了Rails.

###3. ActiveRecord
Yii實作了ActiveRecord,講到ActiveRecord不得不講到Rails,Rails便是採用ActiveRecord模式來做為ORM框架,ActiveRecord Class代表資料庫中的一張表,每個ActiveRecord實體都對應了一筆記錄,ActiveRecord模式的特性讓我們可用快速的採用物件導向的方式來控制資料庫,ActiveRecord可以說是Yii跟Rails的ORM類,但是ActiveRecord跟一般所說的ORM如Java的Hibernate又有很多不同,它實作起來更為快速,直覺。

###4. Unit Test
TDD一直都是很多人在推廣的開發方式,Yii本身支援了基於PHPUnit以及Selenium的單元測試,在Gii產生Model的ActiveRecord類別後會自動產生一個相對應的測試類別.

###5. Automatic Code Generation
Yii擁有相當程度的自動產出功能,建立好一張Table後,便可以採用內建的產生器,產出初期MVC雛形,整個CRUD流程,包含Model,Controller,到前端的表單,都可以在一瞬間建立,省去初期煩人的建置時間.

Yii框架還有相當多的特性,將會在往後的文章裡面提到,另外Yii是由居住於華盛頓DC的華人Qing Xue所設計的PHP框架,這在這個圈子是很難得的,而Yii也已經累積了不少使用者,希望這個框架能夠繼續的發展下去,成為PHP其中一個優秀的App Framework.