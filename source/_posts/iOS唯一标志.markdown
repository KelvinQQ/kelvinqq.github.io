---
title: "iOS唯一标志"
date: 2014-07-19 21:33:48 +0800
tags: 
    - UDID
    - 唯一标志
categories:
    - iOS
 
---
# 背景
由于iOS对于用户隐私的保护,使得在iOS下面想要拿到一个唯一的设备号是不怎么方便的.当然在IOS5,6中还是可以有那么一丝希望的,UUID,MAC地址等.可是后来,苹果彻底把所有的路都堵死了.瞬间大家都觉得没戏了.
# 新方案
既然我写这个博客,说明还是有方法的.大家都知道在Mac OSX上有Keychain,可以保存用户的所有密码.其实在iOS上也有这个Keychain的,只不过没有像Mac OSX上面那样的App来管理罢了.
所以,我们就可以借用这个Keychain来构造设备的唯一标志.
至于iOS底层的库我就不多说了,直接推荐一个三方开源库,[SSKeychain](https://github.com/soffes/sskeychain),大家可以去GitHub下载,也可以使用CocosPods来管理.

<!--more-->

# API
我们主要使用`SSKeychain`中的几个API.
## a.获取密码

```
+ (NSString *)passwordForService:(NSString *)serviceName account:(NSString *)account;
+ (NSString *)passwordForService:(NSString *)serviceName account:(NSString *)account error:(NSError **)error;
```

## b.删除密码

```
+ (BOOL)deletePasswordForService:(NSString *)serviceName account:(NSString *)account;
+ (BOOL)deletePasswordForService:(NSString *)serviceName account:(NSString *)account error:(NSError **)error;
```

## c.设置密码

```
+ (BOOL)setPassword:(NSString *)password forService:(NSString *)serviceName account:(NSString *)account;
+ (BOOL)setPassword:(NSString *)password forService:(NSString *)serviceName account:(NSString *)account error:(NSError **)error;
```

当然,我们是需要一个唯一标志,所以就需要生成一个UUID,这样就可以用`SSKeychain`来设置密码为生成的UUID,具体操作如下:

### 1.创建一个NSString的Category生成UUID

```
@implementation NSString (Extension)
+ (NSString *)stringForUUID
{
   CFUUIDRef uuidObj    = CFUUIDCreate(nil);
   NSString *uuidString = (NSString *)CFBridgingRelease(CFUUIDCreateString(nil, uuidObj));
   CFRelease(uuidObj);
   return uuidString;
}
@end
```

### 2.创建一个HZKeychainManager

```
@implementation HZKeychainManager
+ (NSString *)appUUID
{
  NSError *error = nil;
  NSString *uuid = [SSKeychain passwordForService:@"com.company.app.service" account:@"com.company.app.account" error:&error]; // 获取 密码
  if (!uuid.length) { // 如果获取不到 则新保存一个
      uuid = [NSString stringForUUID];
      BOOL res = [SSKeychain setPassword:uuid forService:kDBServiceKey account:kDBAccountKey error:&error];
      if (!res) {
      }
  }
  return uuid;
}
@end
```