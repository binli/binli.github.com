---
layout: post
published: true
category: worklog
title: mutt msmtp offlineimap
---

Offlineimap
===
喜欢它是因为，目录和邮件服务器的上目录结构一致，而getmail还需要定义一套规则来分类邮件。   
再一个是配置起来容易，可以方便的配置多个邮箱。

	[general]
	accounts = bili, libin

	[Account bili]
	localrepository = bili_local
	remoterepository = bili_remote
	autorefresh = 10

	[Repository bili_local]
	type = Maildir
	localfolders = /work/mail/bili/
	keepalive = 60
	
	[Repository bili_remote]
	type = IMAP
	remotehost = xxxx
	remoteport = xxx
	remoteuser = xxxxxx
	remotepass = xxxx
	
	[Account libin]
	localrepository = libin_local
	remoterepository = libin_remote
	autorefresh = 20
	
	[Repository libin_local]
	type = Maildir
	localfolders = /work/mail/libin/
	keepalive = 60
	
	[Repository libin_remote]
	type = Gmail
	realdelete = yes
	remoteuser = xxx@gmail.com
	remotepass = xxxx

需要注意的是在收取Gmail邮件时一定要把'All Mail', 'Important'的邮件过滤掉，否则，这2个标签下的邮件非常多，同步起来是要死人的，而且是和Inbox下是重复的。

folderfilter = lambda foldername: foldername not in ['[Gmail]/Starred', '[Gmail]/Important', '[Gmail]/All Mail']

参考
===
推荐这篇博客，写的比较简洁，网上搜一下关键字会出来一大堆相关的配置的。

<http://www.imtxc.com/blog/2012/04/22/use-gmail-plus-mutt-plus-msmtp-plus-offlineimap/>
