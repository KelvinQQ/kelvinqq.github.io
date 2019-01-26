---
layout: post
title: "iOS配置gitignore文件"
date: 2014-08-04 22:25:49 +0800
tags: 
    - Git
categories:
    - iOS
---

iOS开发中如果使用Git来管理源代码,很多文件是不需要上传到服务器的,这时候我们就需要配置`.gitignore`文件.具体做法是:

<!--more-->

```
# Xcode
.DS_Store
*/build/*
*.pbxuser
!default.pbxuser
*.mode1v3
!default.mode1v3
*.mode2v3
!default.mode2v3
*.perspectivev3
!default.perspectivev3
xcuserdata
profile
*.moved-aside
DerivedData
.idea/
*.hmap
*.orig

# CocoaPods
Pods
```

<!--more-->

拷贝上述代码到记事本中,然后保存到你的项目根目录下,文件名为`.gitignore`.注意,不要有`.txt`等后缀.该文件是隐藏的,如果需要显示Mac隐藏文件,如下命令即可:

```
defaults write com.apple.finder AppleShowAllFiles -bool true
```

输完单击Enter键，退出终端，重新启动Finder就可以了

重启Finder：按住`option`,鼠标左击`dock`上的`Finder`图标不松,直到出现菜单后点击`重新开启`即可.


顺便附上隐藏Mac隐藏文件命令:

```
defaults write com.apple.finder AppleShowAllFiles -bool false
```