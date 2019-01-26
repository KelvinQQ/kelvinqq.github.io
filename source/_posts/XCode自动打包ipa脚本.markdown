---
title: "Xcode自动打包ipa脚本"
date: 2014-08-11 20:37:31 +0800
tags: 
    - shell 
    - 自动打包
categories:
    - iOS

---

首先申明转载:[http://webfrogs.me/2012/09/19/buildipa/](http://webfrogs.me/2012/09/19/buildipa/).

iOS打包我们可以借助shell脚本来自动完成这项功能.具体做法如下:
<!--More-->

* 复制下面代码到文本中并保存为`ipa_build.sh`.

```
#!/bin/bash

#--------------------------------------------
# 功能：为Xcode工程打ipa包
# 作者：ccf
# E-mail:ccf.developer@gmail.com
# 创建日期：2012/09/24
#--------------------------------------------


#参数判断
if [ $# != 2 ] && [ $# != 1 ];then
    echo "Number of params error! Need one or two params!"
    echo "1.path of project(necessary) 2.name of ipa file(optional)"
    exit    
elif [ ! -d $1 ];then
    echo "Params Error!! The first param must be a dictionary."
    exit    
fi

#工程绝对路径
cd $1
project_path=$(pwd)
#build文件夹路径
build_path=${project_path}/build

#工程配置文件路径
project_name=$(ls | grep xcodeproj | awk -F.xcodeproj '{print $1}')
project_infoplist_path=${project_path}/${project_name}/${project_name}-Info.plist
#取版本号
bundleShortVersion=$(/usr/libexec/PlistBuddy -c "print CFBundleShortVersionString" ${project_infoplist_path})
#取build值
bundleVersion=$(/usr/libexec/PlistBuddy -c "print CFBundleVersion" ${project_infoplist_path})
#取bundle Identifier前缀
#bundlePrefix=$(/usr/libexec/PlistBuddy -c "print CFBundleIdentifier" `find . -name "*-Info.plist"` | awk -F$ '{print $1}')


#IPA名称
if [ $# = 2 ];then
ipa_name=$2
fi


#编译工程
cd $project_path
xcodebuild || exit

#打包
cd $build_path
target_name=$(basename ./Release-iphoneos/*.app | awk -F. '{print $1}')
if [ $# = 1 ];then
ipa_name="${target_name}_${bundleShortVersion}_build${bundleVersion}_$(date +"%Y%m%d")"
fi

if [ -d ./ipa-build ];then
    rm -rf ipa-build
fi
mkdir -p ipa-build/Payload
cp -r ./Release-iphoneos/*.app ./ipa-build/Payload/
cd ipa-build
zip -r ${ipa_name}.ipa *
rm -rf Payload
```

* 为文件添加可执行权限,命令如下:

```
chmod +x ipa_build.sh
```

* 使用如下命令开始打包.

```
./ipa_build ....../ProjectDir Project
```
其中的两个参数分别为:项目路径和最终打包的ipa名.

执行完命令后会在`ProjectDir`文件夹下生成`build`文件夹,ipa文件就在`build`下的`ipa-build`文件夹中.