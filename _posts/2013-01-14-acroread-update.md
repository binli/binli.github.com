---
layout: post
published: true
category: worklog
title: Update acroread!
---
准备升级 acroread 到9.5.3(bnc#797529)，顺便把以前需要fix的bug也都过了一遍。
* acroread 运行后cpu占用100%i (bnc#768492)，与nppdf.so有关，当删除该文件，问题消失。
* 9.5.1以后的版本在安装后会生成 C:\nppdf32Log\debuglog.txt (bnc#757393)，在Linux生成一个windows的路径文件看着就不爽，好像也与nppdf.so有关。
* 在64位平台上libcanberra-gtk-module 的包没有安装会提示下面错误，添加依赖关系。
    Gtk-Message: Failed to load module "canberra-gtk-module":
    libcanberra-gtk-module.so: cannot open shared object file: No such file or directory
* 分离 nppdf.so 到 acroread-browser-plugin 包，用户根据自己需要来安装，并且发现问题时可以单独删除。

