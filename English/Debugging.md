# Tools and Debugging

## Mediation Test Suite
The Yumi Mediation Test Suite allows you to test whether you have correctly configured your application and ad units to be able to display ads from third-party networks via Yumi mediation.

This guide outlines how to use the Yumi Mediation Test Suite in your iOS app. The first step is integrating the tool into your app.

### Prerequisites
- Use Xcode 10 or higher.
- Target iOS 8.0 or higher.

### Installation
- CocoaPods(preferred)
```ruby
pod "YumiMediationDebugCenter-iOS" 
```

- Manually Integrating YumiMediationSDK
[SDKDownloadPage-iOS](https://github.com/yumimobi/YumiMediationSDKDemo-iOS/blob/master/normalDocuments/iOSDownloadPage.md)

Unzip the downloaded file to get our ``YumiMediationDebugCenter-iOS.framework``. Select this framework and add them to your project. Make sure to have 'Copy Items' checked.

### Call debug mode

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

### Sample

<img src="resources/debug-1.png" width="240" height="426">

Select platform integration category

<img src="resources/debug-2.png" width="240" height="426">

Select single platform, the grey indicates  not configurated yet.

<img src="resources/debug-3.png" width="240" height="426">

select ad category, debug single platform

## Test ID
| Formats             | Slot(Placement) ID                                                                                                                | Note                                                                                                                              |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| Banner                 | l6ibkpae                                                                                                                          | Using this test ID, you are able to get test ads which are from YUMI, AdMob, AppLovin, Baidu, Facebook, GDTMob                                         |
| Interstitial  | onkkeg5i | Using this test ID, you are able to get test ads which are form YUMI, AdMob, Baidu, Chartboost, GDTMob, IronSource, InMobi, IQzone, Unity Ads, Vungle, ZPLAYAds                                         |
| Rewarded Video         | 5xmpgti4                                                                                                                          | Using this test ID, you can get test ads which are from YUMI, AdMob, AdColony, AppLovin, IronSource, InMobi, Mintegral, Unity Ads, Vungle, ZPLAYAds                                         |
| Native                 | atb3ke1i                                                                                                                          | You can get test ads which are from YUMI, AdMob, Baidu, GDTMob, Facebook by using this test ID                                                                              |
| Splash                 | pwmf5r42                                                                                                                         | You can get test ads which are from YUMI,Baidu,GDTMob,AdMob,BytedanceAds by using this test ID                                                                                                     |

## Q&A
If you meet any issue when integrating, click to see [YumiMediationSDK Q&A](https://github.com/yumimobi/Developer-doc/blob/master/FAQ_latest_en.md).