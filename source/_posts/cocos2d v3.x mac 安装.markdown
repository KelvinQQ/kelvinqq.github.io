---
title: "cocos2d v3.x mac 安装"
date: 2014-08-08 20:46:28 +0800
tags: 
    - cocos2d
categories:
    - cocos2d-x

---

cocos2d v3.x 版本出来后,从配置安装到创建项目都是命令行,下面简单说一下.

* 官网下载最新版本Cocos2d-x,地址是 [http://cn.cocos2d-x.org/download/](http://cn.cocos2d-x.org/download/).
* 解压后,在命令行中`cp`到文件夹,然后执行`./setup.py`,回车.
* 期间会有几次询问,是设置安卓SDK路径的,不设置安卓直接`Enter`跳过即可

```
 ->Please enter the path of NDK_ROOT (or press Enter to skip):
 ->Please enter the path of ANDROID_SDK_ROOT (or press Enter to skip):
 ->Please enter the path of ANT_ROOT (or press Enter to skip):
```
之后就OK了,会有提示:
```
Please execute command: "source /Users/history/.bash_profile" to make added system variables take effect
```
<!--more-->
根据提示敲击`source /Users/history/.bash_profile`后`Enter`,这样就算设置好了.

* 最后就是创建工程.继续命令行.
`cd tools/cocos2d-console/bin`,接着使用下面命令即可:
`cocos new 工程名 -p 包名 -l 语言 -d 目标文件夹`,例如
`./cocos new HelloWorld -p com.history.HelloWorld -l cpp -d ~/Wrok/Projects/Privates/`.
执行后就有如下提示:

```
Running command: new
> Copy template into /Users/history/Wrok/Projects/Privates/HelloWorld
> Copying cocos2d-x files...
> Rename project name from 'HelloCpp' to 'HelloWorld'
> Replace the project name from 'HelloCpp' to 'HelloWorld'
> Replace the project package name from 'org.cocos2dx.hellocpp' to 'com.history.HelloWorld'
```

这样就大功告成了.