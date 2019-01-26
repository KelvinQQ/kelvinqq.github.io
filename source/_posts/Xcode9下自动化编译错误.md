---
title: Xcode9下自动化编译错误
date: 2017-11-09 22:03:20
tags: [xcodebuild, Jenkins, 自动打包, Xcode9]
categories: iOS
---

最近在使用`CI`平台打包时突然失败了，查看日志后发现是在`exportArchive`时失败了。之前一直都是好好地，升级了`Xcode`之后突然就不行了，提示如下信息：
```
error: exportArchive: "AppName.app" requires a provisioning profile with the Push Notifications and App Groups features.
Error Domain=IDEProvisioningErrorDomain Code=9
"AppName.app" requires a provisioning profile with the Push Notifications and App Groups features." UserInfo={NSLocalizedDescription="AppName.app" requires a provisioning profile with the Push Notifications and App Groups features., NSLocalizedRecoverySuggestion=Add a profile to the "provisioningProfiles" dictionary in your Export Options property list.}
```
<!--more-->
查阅资料后发现，在`Xcode9`下，`xcodebuild`需要配置更多的信息才能导出`ipa`，最主要的一个就是`provisioningProfiles`。
具体的操作步骤如下。
1. 使用`Xcode` `Archive`一个新的版本
![1.png](http://upload-images.jianshu.io/upload_images/606479-78a2269f27ebb3ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 在`Organizer`中找到刚刚`Archive`出来的版本，选择`Export`。
![2.png](http://upload-images.jianshu.io/upload_images/606479-7d463a8e3b69181a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 选择你要导出的`ipa`类型，如果你需要不同版本，可以重复该流程，就可以得到其他类型所需要的信息了。
![3.png](http://upload-images.jianshu.io/upload_images/606479-ac0a0a0c788747a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4. 导出`ipa`到目录
![4.png](http://upload-images.jianshu.io/upload_images/606479-7c81d9a2d4c0f347.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5. 最终导出的目录下会有`4`个文件，除了`ipa`文件还有一个`ExportOptions.plist`文件，这个文件就是我们使用`xcodebuild -exportArchive`命令时，`-exportOptionsPlist`参数需要指定的`plist`文件。
![5.png](http://upload-images.jianshu.io/upload_images/606479-da5c33fac97fc3ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们用这个新的`plist`文件就可以了。如果你需要打其他类型的`ipa`，可以重复上述步骤，在`第三步`重新选择即可。你也可以按照刚才导出的`plist`自己修改。

新的`plist`中有如下一些选项，你也可以参照修改。
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>compileBitcode</key>
    <false/>
    <key>method</key>
    <string>ad-hoc</string>
    <key>provisioningProfiles</key>
    <dict>
        <key>com.tsing.calculate</key>
        <string>calculate_adhoc</string>
    </dict>
    <key>signingCertificate</key>
    <string>iPhone Distribution</string>
    <key>signingStyle</key>
    <string>manual</string>
    <key>stripSwiftSymbols</key>
    <true/>
    <key>teamID</key>
    <string>CL32FD34</string>
    <key>thinning</key>
    <string>&lt;none&gt;</string>
</dict>
</plist>
```
