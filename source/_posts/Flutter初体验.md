---
title: Flutter初体验
date: 2019-01-26 13:55:40
tags: 
    - Flutter
categories:
    - Flutter
---


> * 在我的`iPhone 8`上，列表滑动还是比较卡的。
> * 用`VS Code`好像代码提示不够智能。
> * 嵌套是真的醉了。

# 开发环境搭建

> 这里介绍的是`Mac`平台下的开发环境搭建，使用的`IDE`是`VS Code`。如果你使用的是`Windows`或`Linux`，可以查看[Flutter中文网](https://flutterchina.club/setup-windows/)上的介绍。

<!--more-->

## 安装`Flutter`

```
git clone -b beta https://github.com/flutter/flutter.git
export PUB_HOSTED_URL=https://pub.flutter-io.cn //国内用户需要设置
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn //国内用户需要设置
export PATH=`pwd`/flutter/bin:$PATH
```

上面在命令行中设置的环境变量，只是针对当前的会话。所以我们需要将他们写道系统配置中。

`open $HOME/.bash_profile`

如果目录下不存在改文件，则手动创建一个。然后将下面的代码粘贴进去：

```
export PUB_HOSTED_URL=https://pub.flutter-io.cn //国内用户需要设置
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn //国内用户需要设置
export PATH=PATH_TO_FLUTTER_GIT_DIRECTORY/flutter/bin:$PATH
```
其中`PATH_TO_FLUTTER_GIT_DIRECTORY`是你`clone`的`flutter`的目录，需要你自己替换掉。

运行`source $HOME/.bash_profile`。

> 注意: 如果你使用的是zsh，终端启动时 ~/.bash_profile 将不会被加载，解决办法就是修改 ~/.zshrc ，在其中添加：source ~/.bash_profile

## iOS设置

```
brew update
brew install --HEAD libimobiledevice
brew install ideviceinstaller ios-deploy cocoapods
pod setup
```
如果你之前没有用过`cocoapods`，需要设置一下镜像。命令行中运行：

```
gem sources -l
```

查看当前使用的`RubyGems`源。

```
gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
```

后面的`--remove`附带的参数将上一步查看到的源写上，然后再查看一下`RubyGems`源是否正确，确保如下所示：

```
$ gem sources -l
*** CURRENT SOURCES ***

https://gems.ruby-china.com/
```

如果还有其他关于`RubyGems`的问题，可以到 [https://gems.ruby-china.com/](https://gems.ruby-china.com/) 获得帮助。

## 配置`VS Code`

* 安装`VS Code`

你可以去官网下载，然后安装。这里是[传送门](https://code.visualstudio.com/)。

* 安装`Flutter`插件。

打开后，点击左边侧边栏的插件按钮，搜索`Flutter`插件。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190119210415163.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3pxMjgyNTAyNTMy,size_16,color_FFFFFF,t_70)
第一个就是`Flutter`，安装`Flutter`同时会安装`Dart`插件，等安装完毕后，再`reload`一下就可以了。

## 第一个`Flutter`工程
打开你的`VS Code`，`View->Command Palette`，输入`flutter`并选择`Flutter: New Project`，输入一个工程名，就叫`hello_flutter`吧。

> 名称只能是小写字母和下划线，是不是很变态啊。

回车后选择一个目录，工程就创建好了。

在`VS Code`下方的输出区域，给我们提供了`TERMINAL`功能，连接你的手机，在`TERMINAL`中敲下`flutter run`，就会看到正在编译了。完成后，就能在手机上看到你的第一个`flutter`应用了。

> 这里需要先配置`Xcode`以及证书相关。

至此，你就可以开启你的`flutter`之旅了。你可以按照[这篇文章](https://flutterchina.club/get-started/codelab/)来修改你的工程，创建一个列表来体验。

##### 参考文章
1. [`Flutter`中文网](https://flutterchina.club/)
2. [`Flutter`不一样的跨平台解决方案](https://github.com/yang7229693/flutter-study/blob/master/post/1.%20Flutter%20%E4%B8%8D%E4%B8%80%E6%A0%B7%E7%9A%84%E8%B7%A8%E5%B9%B3%E5%8F%B0%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88.md)
