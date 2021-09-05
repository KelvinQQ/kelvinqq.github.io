---
title: 小程序利用Canvas绘制图片和竖排文字
date: 2018-05-14 21:53:44
tags: [小程序, Canvas]
categories: [小程序]
---

# 引言
闲暇时间抽个空写了个三国杀武将手册的小程序，中间有个需求设计的是合成武将皮肤图、竖排的武将姓名、以及小程序码，然后提供保存图片到相册，最终让用户可以分享到朋友圈或其他平台。合成图片应该按照`Canvas`的文档来做都没什么问题，主要是有个竖排文字的需求，这里和大家分享一下。

<!--more-->

# 广告
宣传一下自己的小程序，有喜欢三国杀的可以关注一下，有什么问题也可以加我微信一起讨论。

`扫码体验小程序`

![](https://user-gold-cdn.xitu.io/2018/5/14/1635f1e3b5c1ccc4?w=258&h=258&f=jpeg&s=52521)

`扫码添加好友，请备注：三国杀`

![](https://user-gold-cdn.xitu.io/2018/5/14/1635f1ee2a33cc4e?w=564&h=786&f=jpeg&s=59275)

# 正文
首先放一张最终保存到相册的图片吧~

![](https://user-gold-cdn.xitu.io/2018/5/14/1635f1f27875f56d?w=640&h=640&f=jpeg&s=69607)

> 自我感觉良好，至少达到了我自己的预期吧~~~

下面让我们一步一步来看看如何实现的吧。

整个图片分为三个部分：

1. 武将图片
2. 小程序码
3. 武将文字信息

先来看一下`wxml`里面的代码，主要是放了一个`canvas`标签，控制了一下高度和宽度属性。

```
<view>
  <canvas class='share-canvas' style="width:100%;height:{{canvasHeight}}px" canvas-id="share_canvas"></canvas>
</view>
```

### 武将图片

```
drawHeroImage: function (path) {
    var that = this;
    // 拿到canvas context
    let ctx = wx.createCanvasContext('share_canvas');
    // 为了保证图片比例以及绘制的位置，先要拿到图片的大小
    wx.getImageInfo({
      src: path,
      success: function (res) {
			
		  // 计算图片占比信息	
        let maxWidth = Math.min(res.width, that.data.canvasWidth * 0.65);
        let radio = maxWidth / res.width;

        let offsetY = (that.data.canvasHeight - res.height * radio) / 2;
        console.log('offsetY=' + offsetY);
        that.setData({
          imageWidth: res.width * radio,
          imageHeight: res.height * radio,
          offsetY: offsetY,
        });
        
        // 绘制canvas背景，不属于绘制图片部分
        ctx.setFillStyle('white')
        ctx.fillRect(0, 0, that.data.canvasWidth, that.data.canvasHeight);
        // 绘制武将图片，path是本地路径，不可以传网络url，如果是网络图片需要先下载
        ctx.drawImage(path, 10, offsetY, res.width * radio, res.height * radio)
        // 绘制小程序码
        that.drawQrCodeImage(ctx);
        // 绘制势力汉字：吴
        that.drawInfluence(ctx, that.data.hero.HERO.INFLUENCE);
        // 绘制武将姓名：陆逊
        that.drawName(ctx, that.data.hero.HERO.NAME);
        // 绘制武将称号：江陵侯
        that.drawHorner(ctx, that.data.hero.HERO.HORNER);
        // 最终调用draw函数，生成预览图
        // 一个坑点：只能调用一次，否则后面的会覆盖前面的
        ctx.draw();
      }
    });
  }
```
  
### 小程序码
小程序码和武将图片是一个类型，无非就是需要计算绘制的位置，这里就不再展示相关代码了。

### 武将文字信息
从刚刚的代码可以看出，我分了3个部分来绘制，其实`吴`和`陆逊`应该是可以放到一起的，但是我在绘制的时候发现，空格在绘制的时候会引起异常，导致空格后面的文字无法绘制出来，所以我这里`吴`和`陆逊`中间的空白是靠位置偏移来做的。

这里就展示一下如何绘制武将称号的。

```
// 绘制武将称号：江陵侯
drawHorner: function (ctx, text) {
	// 设置字号
    ctx.setFontSize(26);
    // 设置字体颜色
    ctx.setFillStyle("#000000");
    // 计算绘制起点
    let x = this.data.offsetX + 35;
    let y = this.data.offsetY + 10;
    console.log('drawHorner' + text);
    console.log(x);
    console.log(y);
    // 绘制竖排文字，这里是个Util函数，具体实现请继续看
    Canvas.drawTextVertical(ctx, text, x, y);
  }
```

绘制竖排文字从网上找了个开源的代码，需要看原理的请看[这里](http://www.zhangxinxu.com/wordpress/?p=7362)

当然我这里为了适用小程序做了些改动，函数原型是这样子的：

```
CanvasRenderingContext2D.prototype.letterSpacingText = function (text, x, y, letterSpacing)
```

原谅我不是很会`js`，完全不懂这是个什么语法，看了一会没弄懂，感觉像是给类添加新的属性，不管他。

> 不管白猫黑猫，能抓到耗子就是好猫

改造后的函数像下面的样子：

`canvas.js`

```
/**
* @author zhangxinxu(.com)
* @licence MIT
* @description http://www.zhangxinxu.com/wordpress/?p=7362
*/
function drawTextVertical(context, text, x, y) {
  var arrText = text.split('');
  var arrWidth = arrText.map(function (letter) {
    return 26;
    // 这里为了找到那个空格的 bug 做了许多努力，不过似乎是白费力了
    // const metrics = context.measureText(letter);
    // console.log(metrics);
    // const width = metrics.width;
    // return width;
  });
  
  var align = context.textAlign;
  var baseline = context.textBaseline;

  if (align == 'left') {
    x = x + Math.max.apply(null, arrWidth) / 2;
  } else if (align == 'right') {
    x = x - Math.max.apply(null, arrWidth) / 2;
  }
  if (baseline == 'bottom' || baseline == 'alphabetic' || baseline == 'ideographic') {
    y = y - arrWidth[0] / 2;
  } else if (baseline == 'top' || baseline == 'hanging') {
    y = y + arrWidth[0] / 2;
  }

  context.textAlign = 'center';
  context.textBaseline = 'middle';

  // 开始逐字绘制
  arrText.forEach(function (letter, index) {
    // 确定下一个字符的纵坐标位置
    var letterWidth = arrWidth[index];
    // 是否需要旋转判断
    var code = letter.charCodeAt(0);
    if (code <= 256) {
      context.translate(x, y);
      // 英文字符，旋转90°
      context.rotate(90 * Math.PI / 180);
      context.translate(-x, -y);
    } else if (index > 0 && text.charCodeAt(index - 1) < 256) {
      // y修正
      y = y + arrWidth[index - 1] / 2;
    }
    context.fillText(letter, x, y);
    // 旋转坐标系还原成初始态
    context.setTransform(1, 0, 0, 1, 0, 0);
    // 确定下一个字符的纵坐标位置
    var letterWidth = arrWidth[index];
    y = y + letterWidth;
  });
  // 水平垂直对齐方式还原
  context.textAlign = align;
  context.textBaseline = baseline;
}

module.exports = {
  drawTextVertical: drawTextVertical
}
```

### 绘制网络图片
由于网络图片无法直接绘制，所以需要先下载到本地，然后再按住本地图片绘制的流程走一遍。


```
downloadHeroImage: function () {
    // 微信不支持非https的图片下载，这里了个替换
    let url = this.data.hero.HERO.ICON.replace(/http/, "https");
    var that = this;
    wx.downloadFile({
      url: url,
      success: function (res) {
        // 下载成功后拿到图片的路径，然后开始绘制
        var path = res.tempFilePath;
        that.drawHeroImage(path);
      }, fail: function (res) {
        console.log(res)
      }
    });
  }
```

### 保存图片
说了这么多，自然少不了最终的一步，将绘制到`canvas`的图片保存到手机相册，这里需要用户授权，你需要自己处理。

用的是微信给我们提供的接口`wx.canvasToTempFilePath`。需要我们传入起点坐标`(x, y)`和画布大小`(width, height)`以及`canvasId`。


```
saveShareImage: function () {
    wx.showLoading({
      title: '正在保存图片..',
    });
    let that = this;
    wx.canvasToTempFilePath({
      x: 0,
      y: 0,
      width: that.data.canvasWidth,
      height: that.data.canvasHeight,
      canvasId: 'share_canvas',
      success: function (res) {
        wx.saveImageToPhotosAlbum({
          filePath: res.tempFilePath,
          success(res) {
            console.log(res);
            wx.showToast({
              title: '保存到相册成功',
              duration: 1500,
            })
          },
          fail(res) {
            console.log(res)
            wx.showToast({
              title: '保存到相册失败',
              icon: 'fail'
            })
          },
          complete(res) {
            console.log(res)
          }
        })
      }
    })
  }
```

# 开源
本着开源的精神，源码已经放在`Github`上，大家可以去上面查看具体代码。

[https://github.com/HistoryZhang/SgsNoteBook](https://github.com/HistoryZhang/SgsNoteBook)

如果对你有帮助的话，麻烦给个`Star`吧~

点击阅读原文可以查看更好的代码格式~