# 开始使用
本指南适用于希望借助 Yumi SDK 通过 iOS 应用获利的发布商 

要展示广告并赚取收入，第一步是将 Yumi 移动广告 SDK 集成到应用中。集成该 SDK 后，您就可以进而实施一种或多种支持的广告格式。

## 前提条件
- 使用 Xcode 10 或更高版本
- 使用 iOS 8.0 或更高版本
- [获取 demo](https://github.com/yumimobi/YumiMediationSDKDemo-iOS.git)

## App Transport Security
[应用传输安全 (ATS)](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html) 是 iOS 9 中引入的隐私设置功能。默认情况下，系统会为新应用启用该功能，并强制实施安全连接。

就任何 iOS 9 和 iOS 10 设备而言，如果运行的是使用 Xcode 7 或更高版本构建的应用且未停用 ATS，那么均会受到此项更改的影响。这可能会影响您的应用与 Google 移动广告 SDK 的集成。

当不符合 ATS 标准的应用试图在 iOS 9 或 iOS 10 设备上通过 HTTP 投放广告时，系统将显示以下日志消息：

App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure. Temporary exceptions can be configured via your app's Info.plist file.

为确保您的广告不受 ATS 影响，请执行以下操作：

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

![ats_exceptions](resources/ats_exceptions.png)

