---
layout: post
title: "[Yii]Ch.1-1 MVC 架構"
date: 2012-04-25 01:32
comments: true
categories: [yii,php,tutorial]
---

MVC 模式，這個模式的概述，應該是已經講到爛掉的 topic 了，所以這篇我主要想從 Yii 對於 Model-View-Controller 的相關實作方式來切入。

<blockquote class="note">
<p>
如果真的不了解 MVC 模式，我想從 Wikipedia 開始會是不錯的選擇 XD
</p>
</blockquote>	

除了 MVC 模式之外，Yii 還有一個名為 Application 的 front-controller，Application 可稱為 Yii 的統整中心，負責處理並分配 User 所有的 Request，整個 Yii app 的結構可以看下面這張示意圖:

![](/images/yii-static-struct.png)

<!-- more -->

Yii MVC 實作主要在下方這幾個類別

* __Model__ : CActiveRecord < CModel
* __Controller__ : CController , CAction
* __View__ : CController , CViewRenderer , CPradoRenderer

Model
-----
Yii 自行實作了自己的 ORM 類別： __CActiveRecord__ ， 從名字上可以看得出來，它是以 ActiveRecord Design Pattern 做為藍本來建構的。

<blockquote class="note">
<p>
如果你同時也是個 Rails 的開發者，我想對 ActiveRecord Pattern 是在熟悉不過了。  當然，除了 Yii 之外，許多其他的 PHP framework 也有實作 ActiveRecord，包含了 CakePHP、FuelPHP、CodeIgniter 等等。
</p>
</blockquote>

每一個繼承自 ActiveRecord 的類別，都代表資料庫中的一張 table ，而每一個ActiveRecord 的實體都代表著一筆紀錄，資料庫的所有 CRUD 的操作都被實作在 ActiveRecord 類別裡面，因此我們可以用 OO 的方式更輕鬆的來處理我們的資料。

舉例來說，如果我們想新增一筆資料在 Foo 這張資料表中，我們可以透過下面這樣
的方式：

```php
<?php
$foo = new Foo;
$foo -> name = 'Bar';
$foo -> save();
```

Yii 中，透過了 `CDbConnection` 來處理資料庫連線的部分，可以在 `config/main.php` 中設置資料庫連線：

```php config/main.php
<?php
…略
'db' => array(
	'class' => 'system.db.CDbConnection', // 預設是不需要定義 class 的
	'connectionString' => 'sqlite:path/to/dbfile'
)
```

如果是 MySQL 設定：

```php
<?php
…略
'db' => array(
	'connectionString' => 'mysql:host=yourhost;dbname=yourdbname',
	'emulatePrepare' => true,
	'username' => '',
	'password' => '',
	'charset' => 'utf8',
)
```

<blockquote class="note">
<p>
Yii 的資料庫設定中，有一項名為 schemaCachingDuration 的設定參數，當這個參數的值 > 0 時，會打開資料庫 metadata 的快取，如果是在 production 環境下，資料表的結構很少變動了，應該要打開這個設定，以節省讀取資料表結構的時間。
</p>
</blockquote>

Controller
----------
Yii app 的 Controller 如下所示：
```php
<?php
class FooController extend Controller
{
// some code...
}
```
其中 `Controller` 也是繼承自 `CController`，它的位置在 `protected/component/` 資料夾下，所有透過 **yiic** 或是 **gii** 所產生的 Controller 都會繼承自 `Controller` 這個類別，方便定義全站 Controller 共同的方法與變數。

Yii 透過 application 中的 url route 來判斷 Controller 與 Action ，不管是 Controller 或是 Action 都是透過 ID 來判斷，這之後再深入探討，簡單來說如果你希望呼叫 `FooController -> actionBar()` url route 就會是 `foo/bar`。

<blockquote class="note">
<p>
你可以修改 <code>config/main.php</code> 中的 <code>urlManager</code> 來自定義 url route。
</p>
</blockquote>

Controller 的實作部分，可以分為 `CController` 與 `CAction`這兩個模組來解釋，`CController` 類別與其子類別回應使用者的 Request 並將其對應到 Action。 一個使用者發送 Request 到 `actionBar` 時，Controller 會經過下面的流程：

1. **Method-based action** ： 尋找 Controller 中是否有名為 `actionBar` 的 method，如果有，便執行它。
2. **Class-based action**：如果沒有 Method-based action ，便搜尋 **action class map** ，（ class map 定義在 Controller 中的 `actions()` 方法 ) 如果 class map 中有定義名為 Bar 的 `CAction` 類別，便新增一個 Bar 的實體，並執行 `Foo -> run()` 方法。
3. **Call missingAction()**：如果都找不到，將會執行 `missingAction()` 方法，你可以複寫這個方法，預設是拋出 `404 HttpException`。

<blockquote class="note">
<p>
如果使用者並沒有要求任何 Action ， Controller 將回傳 <code>defaultAction</code>，預設是 actionIndex，當然，你也可以指定其他的 action 作為 defaultAction。
</p>
</blockquote>

除了在 Controller 中定義 action 方法之外，我們也可以採用所謂的 **Class-based action** 的方式來定義 action，往後的篇章再做說明。

View
----
Yii 中的 View Render，我們可以透過 `CController::render` 來呼叫 View 檔案，預設的 view 檔案會放在 `protected/views/ControllerID/ 這個資料夾下，因此我們在 action 方法中可以呼叫 render() 來呼叫 View ：

```php
<?php
class FooController extends Controller
{
 public actionBar()
 {
 	$this -> render('bar');
 }
}
``` 

當使用者呼叫 `actionBar()` 時，Controller 會找到在 `protected/views/foo/` 資料夾中的 `bar.php` 並顯示在使用者的畫面上，完成這次的 Request。 在 view script 檔案中，我們可以透過 `$this` 來存取 render view 的 controller 實體，Yii 稱這種方式為 `pull`，我們也可以透過所謂 `push` 的方式，直接在 render 的時候將資料送進 view 檔案中：

```php
<?php
public actionBar()
{
	$this -> render('bar', array(
		'var'=>$value
	));
}
```
以上面的範例來說，我們可以直接在 view 之中使用名稱為 `$var` 的變數：

```php protected/views/foo/bar.php
<?php
	//用 pull 的方式 存取 FooController 的 layout 屬性
	echo $this -> layout;
	
	//存取 controller push 過來的 var 變數
	echo $var;
?>
```

Yii 也支援使用者使用自定義的 template syntax，可以透過繼承 `CViewRenderer` 定義自己設計的特殊 syntax。設計好了 View Renderer 只需要在 `config/main.php` 中設定：

```php protected/config/main.php
<?php
...
	'components'=>array(
		...
		'viewRenderer'=>array(
			'class'=>'path/to/your/ViewRender',
		),
	),
```

<blockquote class="note">
<p>
Yii 本身內建了 PradoViewRenderer，只要在設定檔將 viewRenderer class 設定為 <code>CProdoViewRenderer</code> 就可以使用 <a href="http://www.pradosoft.com/demos/quickstart/?page=Configurations.Templates1">Prado</a> 的 template 語法。
</p>
</blockquote>

再複習一次
--------

使用者的一個 Request 進入Yii 從 `index.php` 開始，然後開啟了一個 `CWebApplication` 透過 url router 導入到正確的 Controller，Controller 管理並選擇使用者呼叫的 **Action**，Action 中，可能開啟調用資料庫的 ActiveRecord 取得資料庫資料，最後將畫面 Render 出來，完成一次 Request。

整個 Yii 對 MVC 的實作概念解說大概到這邊，下次將從 EntryScript 開始深入探究 YiiFramework :)。