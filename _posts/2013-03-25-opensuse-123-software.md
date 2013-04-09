---
layout: post
published: true
category: worklog
title: opensuse 12.3 常用软件汇总
---

Skype 安装
===

国内的skype下载不了，<http://skype.tom.com/download/linux.html>，选择openSUSE的链接后又重定向到首页。    
鼠标放在link上不要点，你会看到<http://skype.tom.com/download/linux/skype-4.1.0.20-suse.i586.rpm>，然后
右键点击复制链接，然后在地址栏中更改skype.tom.com/download -> download.skype.com，即    
<http://download.skype.com/linux/skype-4.1.0.20-suse.i586.rpm>

访问下面的地址也可以获取最新版本    
<http://www.skype.com/go/getskype-linux-suse>

64位的系统还需要安装以下的包    
	sudo zypper in libX11-6-32bit libQtWebKit4-32bit libqt4-32bit libasound2-32bit libpng12-0 xorg-x11-libs libXv1-32bit libXss1-32bit alsa-plugins-pulse-32bit

	sudo rpm -ivh skype-4.1.0.20-suse.i586.rpm

切换运行级别
===
init 好像不能用了，得用systemd相关的命令。

    # systemctl isolate runlevel5.target
    # systemctl isolate runlevel3.target
    # systemctl isolate runlevel6.target
