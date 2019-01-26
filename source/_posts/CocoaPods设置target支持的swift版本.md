---
title: 'CocoaPods设置target支持的swift版本 '
date: 2017-11-24 08:50:33
tags: [Swift4, CocoaPods]
categories: [iOS]
---
上一篇文章说道在`Swift4.0`中如何引用`3.0`版本的第三方库，详见[这篇文章](https://historyzhang.github.io/2017/10/21/Swift4.0%E5%BC%95%E7%94%A83.0%E7%AC%AC%E4%B8%89%E6%96%B9%E5%BA%93/)。但是如果`Pods`中有很多第三方库都只支持`3.0`，一个一个修改恐怕是要累死。而且每次执行`pod update`之后之前设置的都会被重置，恐怕是想死的心都有了。
<!--more-->
有一句是说："懒惰"推动了人类的进步。所以程序猿总是有办法的。
> Talk is cheap, Show me the code
```
platform :ios, '10.0'
use_frameworks!

target 'YourTarget' do
    pod 'SnapKit', '~> 4.0.0'
    pod 'Toast-Swift', '~> 2.0.0'
end

post_install do |installer|
    installer.pods_project.targets.each do |target|
        if target.name == 'Toast-Swift'
            target.build_configurations.each do |config|
                config.build_settings['SWIFT_VERSION'] = '3.2'
            end
        end
    end
end
```
