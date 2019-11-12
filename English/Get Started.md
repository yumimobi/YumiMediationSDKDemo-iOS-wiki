This guide is for publishers who want to monetize an iOS app with Yumi Mobile Ads SDK. 

Integrating the Yumi Mobile Ads SDK into an app is the first step toward displaying ads and earning revenue. Once you've integrated the SDK, you can proceed to implement one or more of the supported ad formats.

## Prerequisites
- Use Xcode 10 or higher
- Target iOS 8.0 or higher
- [GetDemo](https://github.com/yumimobi/YumiMediationSDKDemo-iOS.git)

## App Transport Security
[App Transport Security (ATS)](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html) is a privacy feature introduced in iOS 9. It's enabled by default for new apps and enforces secure connections.

All iOS 9 and iOS 10 devices running apps built with Xcode 7 or higher that don't disable ATS will be affected by this change. This may affect your app's integration with the Yumi Mobile Ads SDK.

The following log message appears when a non-ATS compliant app attempts to serve an ad via HTTP on iOS 9 or iOS 10:

App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure. Temporary exceptions can be configured via your app's Info.plist file.

To ensure your ads are not impacted by ATS, do the following:
```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

![ats_exceptions](resources/ats_exceptions.png)

