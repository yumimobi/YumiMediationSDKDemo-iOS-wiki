# 工具和调试

## 中介功能测试套件
通过 Yumi 中介功能测试套件，您可以测试是否正确配置了应用和广告单元，使其能够通过 Yumi 中介功能展示来自第三方广告联盟的广告。

本指南概要介绍了如何在 iOS 应用中使用 Yumi 中介功能测试套件。第一步是将该工具集成到您的应用中。

### 前提条件
- 使用 Xcode 10 或更高版本
- 使用 iOS 8.0 或更高版本.

### 安装
- CocoaPods(preferred)
```ruby
pod "YumiMediationDebugCenter-iOS" 
```

- 手动集成
[SDKDownloadPage-iOS](https://github.com/yumimobi/YumiMediationSDKDemo-iOS/blob/master/normalDocuments/iOSDownloadPage.md)

将下载好的``YumiMediationDebugCenter-iOS.framework``加入``Xcode``工程即可。 

### 调用调试模式

```objective-c
#import <YumiMediationDebugCenter-iOS/YumiMediationDebugController.h>

[[YumiMediationDebugController sharedInstance] presentWithBannerPlacementID:@"Your BannerPlacementID"
                                                    interstitialPlacementID:@"Your interstitialPlacementID"
                                                           videoPlacementID:@"Your videoPlacementID"
                                                          nativePlacementID:@"Your nativePlacementID"
                                                          splashPlacementID:@"Your splashPlacementID"
                                                                  channelID:@"Your channelID"
                                                                  versionID:@"Your versionID"
                                                         rootViewController:self];//your rootVC
```

### 图示

<img src="resources/debug-1.png" width="240" height="426">

选择平台类型

<img src="resources/debug-2.png" width="240" height="426">

选择单一平台，灰色平台为已添加未配置

<img src="resources/debug-3.png" width="240" height="426">

选择广告类型，调试单一平台

## Test ID
| 广告类型               | Slot(Placement) ID                                                                                                                | 备注                                                                                                                               |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| Banner                 | l6ibkpae                                                                                                                          | YUMI,AdMob,APPlovin,Baidu,Facebook,GDTMob  使用此test id,以上Network平台可测试到对应平台广告                                                 |
| Interstitial  | onkkeg5i | YUMI,AdMob,Baidu,Chartboost,GDTMob,IronSource,Inmobi,IQzone, untiy Ads，vungle, ZPLAYAds 使用此test id,以上Network平台可测试到对应平台广告 |
| Rewarded Video         | 5xmpgti4                                                                                                                          | YUMI,AdMob,Adcolony, APPlovin,IronSource,Inmobi,Mintegral, untiy Ads，vungle, ZPLAYAds 使用此test id,以上Network平台可测试到对应平台广告 |
| Native                 | atb3ke1i                                                                                                                          | YUMI,AdMob,Baidu,GDTMob,Facebook 使用此test id,以上Network平台可测试到对应平台广告                                        |
| Splash                 | pwmf5r42                                                                                                                         | YUMI,Baidu,GDTMob,AdMob,穿山甲 使用 test id，以上Network平台可测试到对应平台广告                                                                                               |

## 接入常见问题
若您在接入过程中遇到任何问题，可点击查看[常见问题解答](https://github.com/yumimobi/Developer-doc/blob/master/FAQ_latest_cn.md)。