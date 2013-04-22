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

把密码明文写到一个单独的配置文件中，如bili.pass或libin.pass。

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
	remotepassfile = /work/mail/bili.pass
	
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
	remotepassfile = /work/mail/libin.pass

需要注意的是在收取Gmail邮件时一定要把'All Mail', 'Important'的邮件过滤掉，否则，这2个标签下的邮件非常多，同步起来是要死人的，而且是和Inbox下是重复的。

folderfilter = lambda foldername: foldername not in ['[Gmail]/Starred', '[Gmail]/Important', '[Gmail]/All Mail']

msmtp的配置文件msmtprc。

	defaults
	logfile ~/.msmtp.log
	tls_certcheck off
	
	# gmail account
	account gmail
	auth on
	host smtp.gmail.com
	port 587
	user libin.charles
	passwordeval cat /work/mail/libin.pass
	from libin.charles@gmail.com
	tls on
	
	account SUSE
	host smtp.novell.com
	tls on
	auth on
	port 25
	from bili@suse.com
	user bili
	passwordeval cat /work/mail/bili.pass
	
	#tls_trust_file /etc/ssl/certs/ca-certificates.crt
	# set default account to use (from above)
	account default : gmail

参考
===
推荐这篇博客，写的比较简洁，网上搜一下关键字会出来一大堆相关的配置的。

<http://www.imtxc.com/blog/2012/04/22/use-gmail-plus-mutt-plus-msmtp-plus-offlineimap/>

多帐号的设置

<http://www.df7cb.de/blog/2010/Using_multiple_IMAP_accounts_with_Mutt.html>
