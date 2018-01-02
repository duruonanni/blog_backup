---
title: Chrome插件突破百度云限速方法
tags:
  - 教程
categories:
  - 教程
  - 百度云
date: 2017-05-29 20:27:00
---

## 写在之前的话
呐,今年6月1日(也就是大后天)开始,[百度就全面强制实行账号实名制][1]了.嗯......  
好吧,闲话不多说,这就奉上迟到的:迂回突破百度云限制方法.🐱‍💻  
- 本教程仅适用于**Chrome内核浏览器**
	- 查看是否为该内核,只要进入`浏览器设置`-`关于页面`,显示`Chromium`,就对啦
	- eg: [centbrowser][4]
	![centbrowser][5]  

本教程参考了:[卡饭论坛:2017 突破百度云限速最新方法][2]  

### 本教程相对原教程的区别:  

- **无需科学上网**进入Google WebStore(插件商店)下载User-Agent Switcher插件;
	- 因为我已经下载并打包好插件了,免翻墙,直接安装即可下载
	- 当前User-Agent Switcher版本为1.0.43_0
	- 下载User-Agent Switcher点 [这里][3]
- **无需安装IDM下载器**
	- 要知道IDM的确很强大,但它并不是一款免费软件哦,建议有需要的同学支持正版;
		- 还是不用吧,正版相当贵(对我来说是)...
	- 所以,这里将使用浏览器自带的下载器;
	- 需要更强大下载功能(断点续传...)的同学,建议安装[eagleget][6]软件;  

### 本教程原理推测:
私以为,百度云对Windows下的浏览器在下载文件超过一定大小和数量会触发强制使用百度云客户端下载,再对客户端的下载限速.而百度云网页版对Mac Safari浏览器更友好,限制更低.  
本教程原理推测是,通过使用插件模拟Safari浏览器获得百度云网页版的更高权限.  

## 开始教程
### 1.下载并解压出`User-Agent Switcher`插件 

插件下载地址: http://storage.live.com/items/AEE68C12565C1619!174699?authkey=AJoh90nl3u6Wj4U 

### 2.打开浏览器**扩展管理**窗口 

![baiduyunjiasu_2][9]
### 3.将解压出的`1.0.43_0.crx`插件拖拽至浏览器**扩展管理**窗口中

拖拽后效果如图:  
![baiduyunjiasu_3][10]
### 4.点选插件,将其设置为**safari**

![baiduyunjiasu_4][11]
### 5.打开网页版百度云,开始下载吧
	
## 该方法优缺点总结
- 优点
	1. 安全: 不用安装什么绿色版破解版之流软件;
	2. 方便: 全程使用浏览器,甚至百度云网盘的客户端都不用安装哦;
	3. 高速: 默认情况下,客户端网速`10-30Kpbs`,此方法'1-3Mpbs'
- 缺点
	-  无法批量下载10个以上文件(应该是百度对浏览器下载做出的限制)
		- 解决方法: 多次少量下载
	- 使用`User-Agent Switcher`插件后,无法正常打开一些网站,eg:[bilibili][7]  
		- 解决办法: 不进行下载时关闭该插件  
	- 并没有使用IDM的多断点下载速度快
		- IDM真的好贵,能有效加速那么多够用啦,就这样啦

喏,就这样~🐱‍👤
如有疑问或指正,欢迎评论或给我发邮件哦.

<!-- 参考链接 -->
[1]: http://www.tmtpost.com/2606559.html "百度网盘6月1日起实行实名制，未认证将遭强退"
[2]: http://bbs.kafan.cn/thread-2088255-1-1.html "突破百度云限速最新方法"
[3]: http://storage.live.com/items/AEE68C12565C1619!174699?authkey=AJoh90nl3u6Wj4U "插件下载地址"
[4]: https://www.centbrowser.com/ "centbrowser"
[5]: http://storage.live.com/items/AEE68C12565C1619!174701?authkey=AJoh90nl3u6Wj4U
[6]: http://www.eagleget.com/ 
[7]: http://www.bilibili.com/ 
[8]: http://storage.live.com/items/AEE68C12565C1619!174703:/baiduyun_1.png?authkey=AJoh90nl3u6Wj4U
[9]: http://storage.live.com/items/AEE68C12565C1619!174707:/baiduyun_2.png?authkey=AJoh90nl3u6Wj4U
[10]: http://storage.live.com/items/AEE68C12565C1619!174708:/baiduyun_3.png?authkey=AJoh90nl3u6Wj4U
[11]: http://storage.live.com/items/AEE68C12565C1619!174709:/baiduyun_4.png?authkey=AJoh90nl3u6Wj4U