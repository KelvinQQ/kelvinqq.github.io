---
title: iOS下更改系统音量
date: 2017-09-30 21:29:38
tags: [音量, MPVolumeView]
categories: iOS
---

`iOS`中，如果想更改系统音量，只有2个方法，一是使用私有方法；二是使用`MPVolumeView`。

私有方法不在我们的讨论范围之列，我们来讨论一下如何使用`MPVolumeView`。

用过一系列的音乐播放器都知道，添加一个`MPVolumeView`在`View`上，然后设置`showsVolumeSlider = YES`，就会有一个`SliderView`，用户滑动时，就能更改系统音量。

这样带来的问题就是，
<!--more-->
1. 会显示一个`MPVolumeView`;

2. 需要手动触发滑动事件;

对于第一个问题很简单，`MPVolumeView`的`hidden`属性设置为`YES`即可；所以主要解决如何模拟用户手动滑动事件即可。

不多说，有了思路后就变得很简单了，下面奉上实现代码。

```
/*
 * 设置音量
 */
- (void)setVolume:(float)value {

    UISlider *volumeSlider = [self volumeSlider];
    self.volumeView.showsVolumeSlider = YES; // 需要设置 showsVolumeSlider 为 YES
    // 下面两句代码是关键
    [volumeSlider setValue:value animated:NO];
    [volumeSlider sendActionsForControlEvents:UIControlEventTouchUpInside];
    [self.volumeView sizeToFit];
}

- (MPVolumeView *)volumeView {
    if (!_volumeView) {
        _volumeView = [[MPVolumeView alloc] init];
        _volumeView.hidden = YES;
        [self.window addSubview:_volumeView];
    }
    return _volumeView;
}
/*
 * 遍历控件，拿到UISlider
 */
- (UISlider *)volumeSlider {
    UISlider* volumeSlider = nil;
    for (UIView *view in [self.volumeView subviews]) {
        if ([view.class.description isEqualToString:@"MPVolumeSlider"]){
            volumeSlider = (UISlider *)view;
            break;
        }
    }
    return volumeSlider;
}

```