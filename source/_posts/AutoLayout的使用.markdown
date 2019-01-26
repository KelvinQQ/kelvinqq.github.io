---
title: "AutoLayout的使用"
date: 2014-07-20 11:01:35 +0800
tags: 
    - iOS8
    - AutoLayout
categories:
    - iOS
---

# 前言
iOS8快要发正式版了,iPhone6也要出新的尺寸了,如果你还没有用Storyboard和AutoLayout那将是一种痛苦.
# 正文
自从iPhone出了不同分辨率的屏幕后,iOS开发者也渐渐开始有了android开发者的痛苦了.当然Apple也提供了新的技术来尽量让开发者没有那么费劲.所以就推出了AutoLayout的技术.由于只能支持到IOS6,所以还是有部分的APP没有使用.不过相信还是大势所趋的.
如果你的App都是使用storyboard或者xib来构建的,直接使用可视化操作就可以很轻松的完成了.但是对于复杂的UI免不了使用coding.这时候你就需要手动的去写那么autolayout代码.虽然autolayout的代码也是比较易理解,但是还是不那么的方便.这里推荐大家一个开源库,封装的还可以.是UIView的一个Category.
GitHub上的主页在[这里](https://github.com/smileyborg/PureLayout).

<!--more-->

平时用的多的API也就是设置宽高,设置和父窗口的相对位置以及设置和兄弟窗口的相对位置.如下:

```
/** Centers the view in its superview. */
- (NSArray *)autoCenterInSuperview;

/** Aligns the view to the same axis of its superview. */
- (NSLayoutConstraint *)autoAlignAxisToSuperviewAxis:(ALAxis)axis;


# pragma mark Pin Edges to Superview

/** Pins the given edge of the view to the same edge of the superview with an inset. */
- (NSLayoutConstraint *)autoPinEdgeToSuperviewEdge:(ALEdge)edge withInset:(CGFloat)inset;

/** Pins the edges of the view to the edges of its superview with the given edge insets. */
- (NSArray *)autoPinEdgesToSuperviewEdgesWithInsets:(ALEdgeInsets)insets;

/** Pins 3 of the 4 edges of the view to the edges of its superview with the given edge insets, excluding one edge. */
- (NSArray *)autoPinEdgesToSuperviewEdgesWithInsets:(ALEdgeInsets)insets excludingEdge:(ALEdge)edge;


# pragma mark Pin Edges

/** Pins an edge of the view to a given edge of another view. */
- (NSLayoutConstraint *)autoPinEdge:(ALEdge)edge toEdge:(ALEdge)toEdge ofView:(ALView *)peerView;

/** Pins an edge of the view to a given edge of another view with an offset. */
- (NSLayoutConstraint *)autoPinEdge:(ALEdge)edge toEdge:(ALEdge)toEdge ofView:(ALView *)peerView withOffset:(CGFloat)offset;
	
# pragma mark Align Axes

/** Aligns an axis of the view to the same axis of another view. */
- (NSLayoutConstraint *)autoAlignAxis:(ALAxis)axis toSameAxisOfView:(ALView *)peerView;

/** Aligns an axis of the view to the same axis of another view with an offset. */
- (NSLayoutConstraint *)autoAlignAxis:(ALAxis)axis toSameAxisOfView:(ALView *)peerView withOffset:(CGFloat)offset;


# pragma mark Match Dimensions

/** Matches a dimension of the view to a given dimension of another view. */
- (NSLayoutConstraint *)autoMatchDimension:(ALDimension)dimension toDimension:(ALDimension)toDimension ofView:(ALView *)peerView;

/** Matches a dimension of the view to a given dimension of another view with an offset. */
- (NSLayoutConstraint *)autoMatchDimension:(ALDimension)dimension toDimension:(ALDimension)toDimension ofView:(ALView *)peerView withOffset:(CGFloat)offset;		

# pragma mark Set Dimensions

/** Sets the view to a specific size. */
- (NSArray *)autoSetDimensionsToSize:(CGSize)size;

/** Sets the given dimension of the view to a specific size. */
- (NSLayoutConstraint *)autoSetDimension:(ALDimension)dimension toSize:(CGFloat)size;

```
还有其他如设置`relationship`的API,大家可自行查看GitHub上的主页.		