---
title: iOS下日志记录
date: 2017-09-24 08:22:30
categories:
    - iOS
tags: 
    - Log
---

`iOS`开发中，一般大家都会自定义一个`DLog`的宏来代替`NSLog`，用来控制`Release`下的`Log`输出。
但是有以下几个弊端：
* 没有日志分级。做过`Android`的都知道，`Android`可以分为5级，`Error`、`Warning`、`Info`、`Debug`、`Verbose`。
* 日志没法记录到文件，`Release`版本无法通过`Log`日志定位问题。
<!--more-->
所以今天就推荐一个第三方库，[CocoaLumberjack](https://github.com/CocoaLumberjack/CocoaLumberjack)，完全满足以上需求，不但如此，还支持以下需求：
* 自定义Log文件的文件数、有效期、缓存大小
```
fileLogger.logFileManager.maximumNumberOfLogFiles = 20;
fileLogger.maximumFileSize = 1024 * 1024 * 5;
fileLogger.rollingFrequency = 60 * 60 * 24;
```
具体使用大家还是看看`GitHub`上的介绍。
现在说一下集成中遇到的问题：
1 . 可以自定义输出`Log`的格式，需要实现`DDLogFormatter`协议，下面提供一个示例：
```
- (NSString *)formatLogMessage:(DDLogMessage *)logMessage {
    
    NSString *logLevel = nil;
    switch (logMessage.flag) {
        case DDLogFlagError:
            logLevel = @"[E]";
            break;
        case DDLogFlagWarning:
            logLevel = @"[W]";
            break;
        case DDLogFlagInfo:
            logLevel = @"[I]";
            break;
        case DDLogFlagDebug:
            logLevel = @"[D]";
            break;
        default:
            logLevel = @"[V]";
            break;
    }
    
    NSString *formatString = [NSString stringWithFormat:@"%@ %@ [@%zd] %@ %@", [logMessage.timestamp descriptionWithLocale:[NSLocale currentLocale]], logLevel, logMessage.line, logMessage.function, logMessage.message];
    return formatString;
}
```

2 . 在单步调试时会发现，很多级别的日志不会立即显示到控制台中。在`DDLogMacros.h`中，我们可以看到以下几个宏定义：
```
#define DDLogError(frmt, ...)   LOG_MAYBE(NO,                LOG_LEVEL_DEF, DDLogFlagError,   0, nil, __PRETTY_FUNCTION__, frmt, ##__VA_ARGS__)
#define DDLogWarn(frmt, ...)    LOG_MAYBE(LOG_ASYNC_ENABLED, LOG_LEVEL_DEF, DDLogFlagWarning, 0, nil, __PRETTY_FUNCTION__, frmt, ##__VA_ARGS__)
#define DDLogInfo(frmt, ...)    LOG_MAYBE(LOG_ASYNC_ENABLED, LOG_LEVEL_DEF, DDLogFlagInfo,    0, nil, __PRETTY_FUNCTION__, frmt, ##__VA_ARGS__)
#define DDLogDebug(frmt, ...)   LOG_MAYBE(LOG_ASYNC_ENABLED, LOG_LEVEL_DEF, DDLogFlagDebug,   0, nil, __PRETTY_FUNCTION__, frmt, ##__VA_ARGS__)
#define DDLogVerbose(frmt, ...) LOG_MAYBE(LOG_ASYNC_ENABLED, LOG_LEVEL_DEF, DDLogFlagVerbose, 0, nil, __PRETTY_FUNCTION__, frmt, ##__VA_ARGS__)
```
你会发现`DDLogError`和其他的宏定义的第一个参数不是很一样，然后找到`LOG_ASYNC_ENABLED`的定义，这样就很明白了，如果你需要立即显示，把`LOG_ASYNC_ENABLED`的定义改为如下即可。
```
#ifndef LOG_ASYNC_ENABLED
    #define LOG_ASYNC_ENABLED NO
#endif
```
3 . 说一下`rollingFrequency`这个属性，看了源码后发现，作者是根据文件的创建时间来处理的，所以就会导致这样的问题，`1号15:00`创建的文件，然后用到`2号15:00`就会重新创建一个文件，所以会导致`2号的Log在15:00`被分为2个文件。

如果需要更高度的自定义，可以去[CocoaLumberjack](https://github.com/CocoaLumberjack/CocoaLumberjack)主页上看一下`README`。
