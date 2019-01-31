---
title: 利用Xcode修改iPhone定位
date: 2019-01-30 20:28:52
tags:
	- 定位
categories:
	- iOS

---

{% img /images/blog/利用Xcode修改iPhone定位/cover.jpg %}

图片来自 泼辣有图 By sleepyeyes

> 写这个话题，自然让人想到会利用这个技术去做一些不是很明亮的事情~哈哈，这个不在我们今天的讨论之中，我只提供一些技术，如有任何后果，我不承担任何责任~

<!--more-->

# 0x00 准备工具

* 一台装了`Xcode`的`Mac`电脑
* 一台`iPhone`(无需越狱)
* 一根数据线


# 0x01 坐标选取

手机的定位是利用`GPS`来获取当前位置的经纬度信息的，手机`App`最终都是根据经纬度信息来判断当前位置是否在合理的区域内。所以我们首先需要知道，合理位置的经纬度是多少。

获取经纬度的工具有许多，高德、百度、谷歌都可以，这里我们以[高德](https://lbs.amap.com/console/show/picker)为例，打开网址后，利用关键字搜索到位置的经纬度如下：

{% img /images/blog/利用Xcode修改iPhone定位/image-1.png %}

得到经纬度信息

> 120.026208,30.279212

# 0x02 编写代码

1. 打开`Xcode`，新建一个工程。
2. 新建一个`gpx`文件，内容如下，并添加到工程。

```
<?xml version="1.0" encoding="UTF-8" ?>
<gpx version="1.1"
    creator="GMapToGPX 6.4j - http://www.elsewhere.org/GMapToGPX/"
    xmlns="http://www.topografix.com/GPX/1/1"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.topografix.com/GPX/1/1 http://www.topografix.com/GPX/1/1/gpx.xsd">
    <wpt lat="30.279212" lon="120.026208">
    </wpt>
</gpx>
```

记得把经纬度替换为上一步从高德网页获取到的。

{% img /images/blog/利用Xcode修改iPhone定位/image-2.png %}

# 0x03 配置工程
1. 在`Xcode`项目中，选择`Edit Scheme...`，然后选择`Run->Options->Core Location`。

2. 选中`Allow Location Simulation`，并在下方的`Defalut Location`中选择刚刚新建的`gps.gpx`文件，如下图。

{% img /images/blog/利用Xcode修改iPhone定位/image-3.png %}

# 0x04 大功告成

连接手机，运行你的程序。由于没有写任何`iOS`相关代码，所以应该是一片白屏。不要在意这个。

不要停止你的程序，直接退到后台，然后打开你的高德地图，你发现你的位置就在阿里巴巴了。

{% img /images/blog/利用Xcode修改iPhone定位/image-4.png %}