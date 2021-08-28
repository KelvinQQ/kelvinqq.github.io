---
title: Tinker集成踩坑指北
date: 2021-08-24 18:06:56
categories: 
  - Android
tags:
  - Tinker
---

## 错误一
> com.tencent.tinker.android.dex.DexException: Unexpected magic: [100, 101, 120, 10, 48, 51, 56, 0] 

这个错误坑了好久，查了官方各种`issue`，试了`N`种方法，都不行。
如果你也一直找不到原因，试一下修改`minSdkVersion<=20`。

以下是相关配置供参考：
```
dependencies {
        classpath 'com.android.tools.build:gradle:3.4.0'
        classpath "com.tencent.bugly:tinker-support:1.1.5"
    }
```

<!--more-->

## 错误二
> 补丁文件缺失必需字段：Created-Time、Created-By、YaPatchType、VersionName、VersionCode、From、To，请检查补丁文件后重试！

注意，需要上传的`patch`包是`outputs->patch->release->patch_signed_7zip.apk`。 如果你传的是`outputs->tinkerPatch`目录下的就会报这个错。

如果你没有生成`patch`包，检查你使用的打包脚本，应该是`gradle`中的`tinker-support->buildTinkerPatchRelease`脚本。用了`tinker->buildTinkerPatchRelease`就不会生成`patch`目录。