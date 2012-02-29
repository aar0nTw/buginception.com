---
layout: post
title: "[OSX]關閉Yahoo!輸入法的一點通服務"
date: 2012-02-29 23:38
comments: true
categories: [mac,osx,yahoo!keykey]
---
Yahoo!輸入法算是Mac上好用的注音輸入法了，但是內建的一點通服務不能關閉，只能在 \` 或是 ~ 擇一當快速鍵，
偏偏 \` 常常用到，~就是homedir…用哪個都不對囧，而且不管在中文還是英文輸入模式都會開啟，真的是擾民。

上網搜尋了一下，都是Windows底下的強制關閉方式，OSX的倒是一篇都沒看到，想想既然windows版的也是用plist當設定檔，Mac上絕對也是一樣吧XD？找了一下，一點通Perference的設定檔是在：

```bash
~/Library/Preferences/com.yahoo.KeyKey.OneKey.plist
```

把內容的：

```xml com.yahoo.KeyKey.OneKey.plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>ShortcutKey</key>
	<string>`</string>
</dict>
</plist>
```

``<string>`</string>`` 的部份改掉即可:D


<!-- more -->

**Update:**

有Ptt板友指出要一定要改成空格換行才行

附上我的:

{% include_code com.yahoo.KeyKey.OneKey.plist lang:xml %}

因為我也沒去試別的改法XDrz