# 横幅
## 初始化及请求横幅

```objective-c
#import <YumiMediationSDK/YumiMediationBannerView.h>
@interface ViewController ()<YumiMediationBannerViewDelegate>
@property (nonatomic) YumiMediationBannerView *yumiBanner;
@end    
@implementation ViewController
//init yumiBanner
//调用 `loadAd:` 方法后，banner 广告位会自动填充，无需重复调用。
//如果您不想自动填充 banner 广告位，可以调用 `disableAutoRefresh` 方法。
- (void)viewDidLoad 
{
    [super viewDidLoad];
    self.yumiBanner = [[YumiMediationBannerView alloc] initWithPlacementID:@"Your PlacementID"
                                                                 channelID:@"Your ChannelID" 
                                                                 versionID:@"Your VersionNumber"
                                                                  position:YumiMediationBannerPositionBottom
                                                        rootViewController:self];
    self.yumiBanner.delegate = self;
    [self.yumiBanner loadAd:YES];
    [self.view addSubview:self.yumiBanner];
}
@end
```

## 设置 Banner 尺寸
```objective-c
/// Represents the fixed banner ad size
typedef NS_ENUM(NSUInteger,YumiMediationAdViewBannerSize) 
{
  /// iPhone and iPod Touch ad size. Typically 320x50.
  kYumiMediationAdViewBanner320x50 = 1 << 0,
  // Leaderboard size for the iPad. Typically 728x90.
  kYumiMediationAdViewBanner728x90 = 1 << 1,
  // Represents the fixed banner ad size - 300pt by 250pt.
  kYumiMediationAdViewBanner300x250 = 1 << 2,
  /// An ad size that spans the full width of the application in portrait orientation. 
  /// The height is typically 50 pixels on an iPhone/iPod UI, and 90 pixels tall on an iPad UI.
  kYumiMediationAdViewSmartBannerPortrait = 1 << 3,
  /// An ad size that spans the full width of the application in landscape orientation. 
  /// The height is typically 32 pixels on an iPhone/iPod UI, and 90 pixels tall on an iPad UI.
  kYumiMediationAdViewSmartBannerLandscape = 1 << 4
};
```
```objective-c
//在 iPhone 上默认为 320 * 50，如无调整不需设置下列代码。
//在 iPad 上默认为 728 * 90，如无调整不需设置下列代码。
//如果您有特殊需求，可根据枚举所述尺寸，在 loadAd 之前，执行下列代码。
self.yumiBanner.bannerSize = kYumiMediationAdViewBanner300x250;
```

## 移除 Banner
```objective-c
  //remove yumiBanner
  - (void)viewWillDisappear:(BOOL)animated {
    [super viewWillDisappear:animated];
    if (_yumiBanner) {
      [_yumiBanner disableAutoRefresh];
      [_yumiBanner removeFromSuperview];
      _yumiBanner = nil;
    }
  }
```

## 实现代理方法
```objective-c
  //implement Yumi Banner delegate
  - (void)yumiMediationBannerViewDidLoad:(YumiMediationBannerView *)adView{
    NSLog(@"adViewDidReceiveAd");
  }
  - (void)yumiMediationBannerView:(YumiMediationBannerView *)adView didFailWithError:(YumiMediationError *)error{
    NSLog(@"adView:didFailToReceiveAdWithError: %@", error);
  }
  - (void)yumiMediationBannerViewDidClick:(YumiMediationBannerView *)adView{
    NSLog(@"adViewDidClick");
  }
```

## 自适应功能
```objective-c
  - (void)loadAd:(BOOL)isSmartBanner;
```

您在请求 `banner` 广告的同时可以设置是否开启自适应功能。

如果设置 `isSmartBanner` 为 `YES` ,YumiMediationBannerView 将会自动根据设备的尺寸进行适配。

此时您可以通过下面的方法获取 YumiMediationBannerView 的尺寸。
```objective-c
  - (CGSize)fetchBannerAdSize;
```

![fzsy](resources/fzsy.png)
<p align="left">非自适应模式</p>  	  

![zsy](resources/zsy.png)
<p align="left">自适应模式</p> 

