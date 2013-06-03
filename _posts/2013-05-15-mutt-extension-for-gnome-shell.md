---
layout: post
published: true
category: worklog
title: mutt extension for gnome shell
---
gnome-shell-extension-mutt 已经基本可用了，贴了2张图看看效果，如果你正好使用mutt和gnome-shell，那么不防试一试。

<https://github.com/binli/gnome-shell-extension-mutt>

![Screenshot with new mails][screenshot]
![Screenshot without new mails][screenshot-nomails]

FAQ
===
收集了一下，在开发过程中碰到的小问题。

1. Where is the system icons?

gnome-icon-theme-symbolic package, include mail-unread-symbolic and mail-read-symbolic.

2. How to add an icon?  
有2种方式，一种是传统的，新建icon，新建button，把icon加到button中，再插入到panel上。

另一种是使用，PanelMenu.SystemStatusButton，推荐使用这个，方便且功能强大，你可以直接操作菜单的相关东东。

3. What's the use of "__proto__:"?  

原型，类似于基类，目前是这么理解的。

TODO
===
1. treeview的添加和删除

[screenshot]: https://raw.github.com/wiki/binli/gnome-shell-extension-mutt/mutt-indicator.png
[screenshot-nomails]: https://raw.github.com/wiki/binli/gnome-shell-extension-mutt/mutt-indicator-nomails.png
