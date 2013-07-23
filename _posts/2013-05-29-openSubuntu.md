---
layout: post
published: true
category: worklog
title: 打造openSubuntu - 从openSUSE 12.3 切换到 ubuntu 13.04 遇到的问题
---
刚切换到ubuntu小有不适，发现有些好的东东在ubuntu上都没有，那么就集两家之长来打造我的openSubuntu。

offlineimap 读取 Gmail 出错 "Server SSL fingerprint"
===
offlineimap 收取不了邮件，找到了相应的bug和解决方案。
具体的原因不知是版本升级的缘故还是其它，在openSUSE上一直无问题。

<https://bugs.launchpad.net/ubuntu/+source/offlineimap/+bug/1015692>

在Repository中加入报错提示的fingerprint就行了

    cert_fingerprint = ""

HOME下无bin 目录
===
自己的rrc恢复脚本应检查一下这个目录是否存在，默认在openSUSE是创建的

Ubuntu中HOME下的.profile中检查该目录并加入到$PATH中。

	if [ -d "$HOME/bin" ] ; then
	    PATH="$HOME/bin:$PATH"
	fi

更新了<https://github.com/binli/script_factory>

常用的别名
===
在openSUSE上有些非常方便的命令

    .. 到上级目录
    ... 到上上级目录
    you 在线更新

查找了一下发现都在下面的文件中

/etc/profile.d/alias.bash

    alias o='less'
    alias ..='cd ..'
    alias ...='cd ../..'
    alias cd..='cd ..'
    
    if test "$is" != "ksh" ; then
        alias -- +='pushd .'
        alias -- -='popd'
    fi
    
    alias rd=rmdir
    alias egrep='egrep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias grep='grep --color=auto'
    alias md='mkdir -p'
    
    alias you='if test "$EUID" = 0 ; then /sbin/yast2 online_update ; else su - -c "/sbin/yast2 online_update" ; fi'
    alias unmount='echo "Error: Try the command: umount" 1>&2; false'

移植到了ubuntu上，ubuntu的别名文件和openSUSE我别名文件的命名方式还略有不同。

<https://github.com/binli/script_factory/blob/master/homerc/bash_aliases>

用quilt 制作patch
===
使用rpm的好处是直接可以使用`quilt setup xxx.spec`来直接解开源码的压缩包。
使用deb包就没这么方便了，得选用`dpkg-source -x xxx.dsc`来解开源码和patch的压缩包。
注意的是deb的patches在debian目录下，进入源码目录后，还得把它链接过来`ln -s debian/patches .`。

这之后两者使用起来没什么区别了，`quilt push`, `quilt new`, `quilt add`, ....
