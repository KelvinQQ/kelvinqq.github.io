---
title: FFmpeg解码流程
date: 2016-09-30 20:11:56
tags: 
	- FFmpeg
categories:
	- FFmpeg

---

## 以下系列文章基于FFmpeg 3.1.1和Xcode8。


#### [FFmpeg iOS库编译与集成](http://historyzhang.github.io/2016/09/26/FFmpeg%20iOS%E5%BA%93%E7%BC%96%E8%AF%91%E4%B8%8E%E9%9B%86%E6%88%90/)

#### [FFmpeg解码流程](http://historyzhang.github.io/2016/09/30/FFmpeg%E8%A7%A3%E7%A0%81%E6%B5%81%E7%A8%8B/)

***

学习 `FFmpeg` ，就不得不提到一位大神，就是 [雷霄骅](http://my.csdn.net/leixiaohua1020)，可惜天妒英才，在这里也先缅怀一下，同时也感谢他在视音频领域以及 `FFmpeg` 解析上做出的贡献。
***
我们先了解一下视频播放的流程，这里主要参考的是雷神的文章，[[总结]视音频编解码技术零基础学习方法](http://blog.csdn.net/leixiaohua1020/article/details/18893769) 。过程见下图（图片同样来自雷神的文章，红色框框是我注解的）。

<!--more-->

![20140201120523046.jpeg](http://upload-images.jianshu.io/upload_images/606479-78a134f68de545b8.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>播放一个互联网上的视频文件，需要经过以下几个步骤：解协议，解封装，解码视音频，视音频同步。
如果播放本地文件则不需要解协议，为以下几个步骤：解封装，解码视音频，视音频同步。

关于每个步骤的含义还是去雷神的文章去看，这里就不啰嗦了。

本文重点讨论的是解封装、解码视频。对于音频的处理先不管。

> **解码**的作用，就是将视频/音频压缩编码数据，解码成为非压缩的视频/音频原始数据。音频的压缩编码标准包含AAC，MP3，AC-3等等，视频的压缩编码标准则包含H.264，MPEG2，VC-1等等。解码是整个系统中最重要也是最复杂的一个环节。通过解码，压缩编码的视频数据输出成为非压缩的颜色数据，例如YUV420P，RGB等等；压缩编码的音频数据输出成为非压缩的音频抽样数据，例如PCM数据。

好了，说了这么多理论，说点实在的。 `FFmpeg` 解码流程所需要调用的 `API` 依次为：

```
开始—->
av_register_all();
avformat_open_input()；
av_find_stream_info();
av_find_best_stream();
avcodec_find_decoder();
while(av_read_frame()) {
    获取到packet—->
    avcodec_send_packet();
    avcodec_receive_frame();
    获取到frame
}
```

上面的流程参考 [笔谈FFmpeg（一）](http://depthlove.github.io/2015/04/27/talk-about-FFmpeg-part1/)，其中有几个函数弃用了，所以我更新了一下。

简单的说一下更新的几个函数，其他的网上介绍的很多了，后面我也会推荐几篇文章。

* `av_find_best_stream()`：

之前用的都是这样的方法：  `穷举所有的流，查找其中种类为CODEC_TYPE_VIDEO` 。所以看别人的文章会有个 `while` 的循环。

* `avcodec_send_packet();avcodec_receive_frame();`：

之前用的是 `avcodec_decode_video2()` 。后来 `FFmpeg` 把函数拆分了。

* 还有个需要注意的，`avcodec_find_decoder();` 步骤中所用到的也有所变动。下面是以前的用法：

```
pCodecCtx = pFormatCtx->streams[videoindex]->codec;  
pCodec = avcodec_find_decoder(pCodecCtx->codec_id);  
```
下面是变动之后的用法：

```
pCodecCtx = avcodec_alloc_context3(NULL);
avcodec_parameters_to_context(pCodecCtx, pFormatCtx->streams[videoStream]->codecpar);
pCodec = avcodec_find_decoder(pCodecCtx->codec_id);
```
通过以上的步骤，获取到 `frame` 数据就是解码后的原始视频数据。后面我们的存储或者播放也都是基于这个数据的。
***
#### 参考文章列表：
*  [100行代码实现最简单的基于FFMPEG+SDL的视频播放器（SDL1.x）](http://blog.csdn.net/leixiaohua1020/article/details/8652605)  作者：[雷霄骅](http://my.csdn.net/leixiaohua1020)

*  [笔谈FFmpeg（一）](http://depthlove.github.io/2015/04/27/talk-about-FFmpeg-part1/) 作者：[Minmin.Sun](http://depthlove.github.io/)

*  [ffmpeg解码流程](http://blog.csdn.net/ownwell/article/details/8113980) 作者：[cyning4星运](http://my.csdn.net/ownWell)