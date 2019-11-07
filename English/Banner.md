# Banner 

## Create and Load Banner Ad View

```objective-c
#import <YumiMediationSDK/YumiMediationBannerView.h>
@interface ViewController ()<YumiMediationBannerViewDelegate>
@property (nonatomic) YumiMediationBannerView *yumiBanner;
@end    
@implementation ViewController
//init Yumi Banner View
//if you implement the method `loadAd:`,the banner placement will auto refresh.You don't need to call this method repeatedly.
//if you don't want to auto refresh banner,You can implement the method `disableAutoRefresh`.
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

## Set Banner Size
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
  //The SDK now supports five banner sizes
  //iPhone and iPod Touch ad size. Typically 320x50.If there are no special requirements, there is no need to execute the code below.
  //Leaderboard size for the iPad. Typically 728x90.If there are no special requirements, there is no need to execute the code below.
  //If you have special requirements, select a size from the enumeration and execute the code below.
  self.yumiBanner.bannerSize = kYumiMediationAdViewBanner300x250;
```

## Remove banner
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

## Delegate implementation 
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

## Self-adaptation
```objective-c
  - (void)loadAd:(BOOL)isSmartBanner;
```

You are available to set whether to turn on self-adaptation when making `banner` request.
If `isSmartBanner` is `YES` ,YumiMediationBannerView will automatically adapt to size of device. 
You are supported to get size of YumiMediationBannerView by the following method.
  
```objective-c
  - (CGSize)fetchBannerAdSize;
```

![fzsy](resources/fzsy.png)
<p align="left">non self-adaptation mode</p>  	  

![zsy](resources/zsy.png)
<p align="left">self-adaptation mode</p> 

