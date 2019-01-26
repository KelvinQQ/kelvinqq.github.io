---
title: Swift关于!和?的Tip
date: 2018-02-06 21:45:07
tags: 
    - Swift
categories:
    - iOS
---


## 前言
> Q: 为什么我现在还在写这些入门级别的语法 `Tip` 呢？
>
> A: 因为我现在才开始学习 `Swift` 呗~
>
> Q: 为什么我现在才开始学习 `Swift` 呢？
>
> A: 因为懒呗~

<!--more-->

哈哈~分享一些自己看到的小 `Tip` 给大家，让那些也才入门 `Swift` 的童鞋也能多了解一些~

## 关于 `!` 和 `?`
今天这里不打算介绍为什么 `Swift` 里面会有 `!` 和 `?`，有什么不明白的可以看看这里：[Swift中 ！和 ？的区别及使用
](https://www.jianshu.com/p/89a2afb82488)

现在我们看一个平时用到的例子：

```
let width: Int? = 3
var area: Int?
    
area = width! * width!
```

为什么这里 `let area = width! * width!` 会有这么多的 `!` 呢？因为机（愚）智（蠢）的 `Xcode` 建议我们这样写。最关键的是如果 `width` 真的就是 `nil` 会这么样呢？机智如你，会 `Crash`。

那保险一点的写法应该是这样子的：

```
let width: Int? = 3
var area: Int?

if let tmp = width {
    area = tmp * tmp
}
else {
    area = nil
}
```
完全不符合 `Swift` 作为一门`优雅`的语言的称号。

## Optional Map
面对上面的问题，有个很优雅的写法，那就是 `Optional Map`。让我们先看一下 `Optional` 中关于 `map` 的声明。

```
public enum Optional<Wrapped> : ExpressibleByNilLiteral {
    public func map<U>(_ transform: (Wrapped) throws -> U) rethrows -> U?
}

```
该方法的作用是，如果输入有值，则进入 `transform` 的闭包进行变化，并返回一个 `U?` ;如果输入就是 `nil` 的话，则直接返回 `nil` 的 `U?`。

那么我们就有如下优雅的写法了：

```
let width: Int? = 3
let area = width.map {
    $0 * $0
}
```

## 更多
我们还能在 `Collection` 中看到 `map` 的身影。

```
extension Collection {

    /// Returns an array containing the results of mapping the given closure
    /// over the sequence's elements.
    ///
    /// In this example, `map` is used first to convert the names in the array
    /// to lowercase strings and then to count their characters.
    ///
    ///     let cast = ["Vivien", "Marlon", "Kim", "Karl"]
    ///     let lowercaseNames = cast.map { $0.lowercaseString }
    ///     // 'lowercaseNames' == ["vivien", "marlon", "kim", "karl"]
    ///     let letterCounts = cast.map { $0.count }
    ///     // 'letterCounts' == [6, 6, 3, 4]
    ///
    /// - Parameter transform: A mapping closure. `transform` accepts an
    ///   element of this sequence as its parameter and returns a transformed
    ///   value of the same or of a different type.
    /// - Returns: An array containing the transformed elements of this
    ///   sequence.
    public func map<T>(_ transform: (Self.Element) throws -> T) rethrows -> [T]
}
```
注释的很清楚了，就不多介绍了。值得注意的是 `Collection` 本身是一个 `protocol`，所以所有实现了 `Collection` 协议的都有这个方法。