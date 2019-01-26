---
title: "iOS汉字转拼音"
date: 2014-12-12 16:58:02 +0800
tags: 
    - 拼音
categories:
    - iOS

---
汉字转拼音之前有很多人用的都是一个拼音库,`pinyin.h`和`pinyin.m`.用着还算方便吧.

后来发现苹果的`framework`提供了方法.于是在这里记录下来.

<!--more-->

主要是用到这个方法:`CFStringTransform`.具体大家可以去头文件看看.这里就贴出代码了.

	- (NSString *)spelling
	{
	    if (self.length) {
	        NSMutableString *copy = [self mutableCopy];
	        CFStringTransform((__bridge CFMutableStringRef)copy, NULL, kCFStringTransformMandarinLatin, NO); // 得到带音调的拼音
	        CFStringTransform((__bridge CFMutableStringRef)copy, NULL, kCFStringTransformStripDiacritics, NO); // 过滤掉音调 每个汉字之间会用空格分开
	        [copy replaceOccurrencesOfString:@" " withString:@"" options:NSCaseInsensitiveSearch range:NSMakeRange(0, copy.length)]; // 过滤掉空格
	        return copy;
	    }
	    else {
	        return nil;
	    }
	}
	
偷偷告诉大家一个神奇的事情,就是这个方法可以准确识别出`重庆`和`重量`.其他待测试.