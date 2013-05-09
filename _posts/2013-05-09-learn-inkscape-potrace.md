---
layout: post
published: true
category: worklog
title: learn inkscape and potrace
---

<http://2013.gnome.asia/tshirts/>

在GNOME.Asia 2013 T恤设计大赛中的我设计的Oppa GNOME Style! 获得了最高的投票，
已经作为今年 GNOME.Asia 的志愿者的衣服，今天组委会索要svg格式，之前是用GIMP
做的，一下子有点头大。

默认的模板为白底，我也是在此基础上做点东东   

* 准备添加GNOME Love的 Logo 和 GANGNAM Style 的元素，但这两个都没找到SVG的文件，使用potrace 转成了SVG   
* 为 Oppa GNOME Style! 使用了Ghostmeat字体，最后用 “Object to Path” 做了下转换，SVG就不依赖于具体的字体了  
* GNOME Love 的 Logo 设计在袖口上，所以中间分开，前后各用一半  
* 最后，选择所有需要反色的Object，转换为黑底的样式  

potrace 的简单用法

	$ convert gnomelover2_left.png gnomelover2_left.pnm         # potrace不支持png，先用convert转换一下
	$ potrace -s -o gnomelover2_right.svg gnomelover2_right.pnm # pnm 转换成 svg
