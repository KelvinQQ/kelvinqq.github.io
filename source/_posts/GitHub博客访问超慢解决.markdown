---
title: "GitHub博客访问超慢解决"
date: 2014-08-01 23:57:58 +0800
tags: 
    - 博客
categories:
    - 杂谈


---

越来越发现博客访问速度超慢了,看了几篇博客说是`google`的问题,就把所有关于`google`的东西都删了,还是很慢.也参照过[唐巧](http://blog.devtang.com/)的技术博客[象写程序一样写博客：搭建基于github的博客](http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github/)中:

* 主要是修改文件：_config.yml ，这个配置文件都有相应的注释。主要就是改一些博客头，作者名之类的东西。 注意最好把里面的twitter相关的信息全部删掉，否则由于GFW的原因，将会造成页面load很慢。
* 修改定制文件/source/_includes/custom/head.html 把google的自定义字体去掉，原因同上。
<!--more-->

可是发现还是很慢.最终测试发现每次加载其实是由于在加载汉字时很慢,加载英文,图片和数字倒是很快,再参考网上所说还是觉得使用了`google`的字体.所以决定还是找个博客看一下他们的字体是怎么设置的.

于是进入了[码农人生](http://msching.github.io)的`GitHub`主页,然后进入`source`分支,找到`msching.github.io / source / _includes / head.html`这个文件,打开后和我的文件对比,发现他主要修改了这里:

		<!--[good job! gfw]><script src="//ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script><!-->
		<script src="http://cdn.staticfile.org/jquery/1.9.1/jquery.min.js"></script>
		<link href="/stylesheets/google-fonts.css" rel="stylesheet" type="text/css">

还写上了注释,于是无耻的偷了过来,不过要注意,第三行中的`.css`文件是本地的引用,在`/stylesheets/google-fonts.css`中,于是来到`msching.github.io / source / stylesheets / google-fonts.css`这里,拷贝下文件内容新建并保存到同样的目录下.

大功告成,上传后试了下,果然速度大幅度提升.