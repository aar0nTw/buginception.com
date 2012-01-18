---
layout: post
title: "[Yii]Assign Attribute's issue"
date: 2012-01-11 15:28
comments: true
categories: [yii]
---
通常情況下，我們習慣這樣來給予Model值

```php FooController
<?php
//....上略
$model = new Foo();
if(isset($_POST['Foo'])){
	$model->attributes = $_POST['Foo'];
}
?>
```
這叫做 __massively assigned__
<!-- more -->

而輸出json時我們會使用CJSON來encode
```php
<?php
//....上略
echo CJSON::encode($model);
?>
```
問題來了,有時我們同時做 __massively assigned__ 再輸出JSON時, 接收的一方(也許是mobile phone) 怎麽parse都parse不出我們encode的JSON

這時候就要注意你的Model有没有問題了,因爲**massively assigned**是不允許**unsafe attribute**的
請檢查你的Model::rules()
```php Model:Foo
<?php
public function rules()
{
	//略
}
```

檢查你的model attribute有無遺漏,因爲有時候因爲新的需求,在db加入新的schema,而忘了在Model裏加入新的rule,這個attribute就會變成unsafe,會在YII_DEBUG出現錯誤訊息,output buffering也會出現長度3的空白string,JSON當然就parse不出來了

我建議在新增column時,還是使用migration,比較安全:)
