---
title: 使用LeanCloud快速开发一款小程序
date: 2019-01-26 14:03:04
tags: 
    - 小程序
    - LeanCloud
categories:
    - 小程序
---

# 前言

开发小程序离不开后台数据，对于独立开发者来说，既要写前端，又要写后端，工作量就会骤然增大。微信提供的云开发无疑是给独立开发者提供了很大的便利，但是由于其数据库不支持联表查询，对于某些场景就不是那么的友好了。当然，市面上有很多的`BaaS`服务提供商，大都类似，今天我们就用其中的一个`LeanCloud`来讲解一下，如何快速使用`LeanCloud`来开发一个小程序。
由于本次重点在`LeanCloud`，所以小程序的开发内容就不是重点。

<!--more-->

# 现在开始
1. 先去[https://leancloud.cn](https://leancloud.cn)官网注册一个账号，然后登录去控制台创建一个新应用。
2. 在微信小程序后台中配置域名白名单，具体需要按照[这里](https://leancloud.cn/docs/weapp-domains.html)说明的来配置，你也可以先跳过这一步，等完全开发完毕后再来配置。可在开发者工具的 **详情** > **项目设置** 中勾选**不校验安全域名、TLS 版本以及 HTTPS 证书**。
3. 下载你熟悉的SDK，目前支持`JS`，`WePY`，`mpvue`，下载链接在[这里](https://leancloud.cn/docs/sdk_setup-js.html#hash649053555)，后面以`JS`来说明，其他方式的`SDK`导入以及使用方法参考文档中的说明。
4. 初始化`SDK`，在`app.js`中加入以下代码即可。`appId`和`appKey`可以在控制台中的应用找到。
```
const AV = require('./utils/av-live-query-weapp-min');

AV.init({
  appId: '换成你自己的appId',
  appKey: '换成你自己的appKey',
});
```
5. 查询数据
先需要在控制台中的应用下新建一个表，在网页中叫做`Class`。每一张表会默认创建`objectId`、`createdAt`、`updatedAt`、`ACL`四个字段，分别表示`数据索引`，`创建时间`，`更新时间`、`权限`。你可以添加你想要的字段，目前支持以下几种类型。
![支持类型](https://img-blog.csdnimg.cn/20181224211110711.png)

其中`Object`是`map`对象，`GeoPoint`是经纬度信息，`Pointer`是另外一张表的表名，做多表联合查询使用的。
假设我们的表名是`T_TODO`，我们可以用以下代码来查询该表下面的数据。
```
new AV.Query('T_TODO')
      .descending('createdAt') // 排序
      .limit(10) // 分页数量
      .skip(10) // 跳过数量
      .find()
      .then(function(results) {
      		that.setData({todo: results})
      })
      .catch(console.error);
  }
  ```
  在你的`WXML`中可以这样写来做数据绑定：
  ```
  <!-- pages/todos/todos.wxml -->
<block wx:for="{{todos}}" wx:for-item="todo" wx:key="objectId">
  <text data-id="{{todo.objectId}}">
    {{todo.content}}
  </text>
</block>
```
是不是很方便。

6. 多表查询
如果需要多多表查询，先要在一张表中新建一个`Pointer`字段，新建时会让你选择指向的表名，如下图所示：
![演示](https://img-blog.csdnimg.cn/20181224215910353.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3pxMjgyNTAyNTMy,size_16,color_FFFFFF,t_70)
然后在查询是使用`include`，就会返回关联表中的所有信息了，如下所示：
```
new AV.Query('T_TODO')
      .descending('createdAt') // 排序
      .limit(10) // 分页数量
      .skip(10) // 跳过数量
      .include('T_POINT_CLASS')
      .find()
      .then(function(results) {
      		that.setData({todo: results})
      })
      .catch(console.error);
  }
  ```
7. 更新对象
小程序中对表中字段做操作后，需要同步更新到服务端，可以使用以下代码来保存对象。
```
  // 第一个参数是 className，第二个参数是 objectId
  var todo = AV.Object.createWithoutData('Todo', '5745557f71cfe40068c6abe0');
  // 修改属性
  todo.set('content', '每周工程师会议，本周改为周三下午3点半。');
  // 保存到云端
  todo.save();
  ```
  8. 其他更多的操作请查看文档，不过你找不到小程序对应的详细开发文档，只能找到[数据存储开发指南 · JavaScript](https://leancloud.cn/docs/leanstorage_guide-js.html#hash810954180)
  
# 广告时间
`三国图鉴`是我业余时间开发的查询三国杀武将技能以及官方活动的小程序，后台服务就是由`LeanCloud`提供的，请大家多多关照。如果有其他问题，你可以关注我的公众号来联系我。

* 扫码体验小程序
![三国图鉴](https://img-blog.csdnimg.cn/20181224221759234.png)
