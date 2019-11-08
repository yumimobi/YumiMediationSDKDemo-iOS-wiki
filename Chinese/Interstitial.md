# 插屏

## 初始化及请求插屏

```objective-c
#import <YumiMediationSDK/YumiMediationInterstitial.h>
@interface ViewController ()<YumiMediationInterstitialDelegate>
@property (nonatomic) YumiMediationInterstitial *yumiInterstitial;
@end

@implementation ViewController
//init yumiInterstitial
//插屏广告位会自动加载广告，您无需重复调用。
- (void)viewDidLoad {
  [super viewDidLoad];
  self.yumiInterstitial =  [[YumiMediationInterstitial alloc] initWithPlacementID:@"Your PlacementID"
                                                                        channelID:@"Your ChannelID"
                                                                        versionID:@"Your VersionID"];
  self.yumiInterstitial.delegate = self;
}
@end
```
## 展示插屏

```objective-c
//present YumiMediationInterstitial
- (IBAction)presentYumiMediationInterstitial:(id)sender {
  if ([self.yumiInterstitial isReady]) {
    [self.yumiInterstitial presentFromRootViewController:self];
  } else {
    NSLog(@"Ad wasn't ready");
  }
}
```

## 实现代理方法

```objective-c
// implementing YumiMediationInterstitial Delegate
// Tells the delegate that the interstitial ad request succeeded.
- (void)yumiMediationInterstitialDidReceiveAd:(YumiMediationInterstitial *)interstitial {
  NSLog(@"YumiMediationInterstitialDidReceiveAd");
}
/// Tells the delegate that the interstitial ad failed to load.
- (void)yumiMediationInterstitial:(YumiMediationInterstitial *)interstitial
           didFailToLoadWithError:(YumiMediationError *)error {
  NSLog(@"YumiMediationInterstitialDidFailToLoadWithError: %@", [error localizedDescription]);
}
/// Tells the delegate that the interstitial ad failed to show.
- (void)yumiMediationInterstitial:(YumiMediationInterstitial *)interstitial
           didFailToShowWithError:(YumiMediationError *)error {
  NSLog(@"YumiMediationInterstitialDidFailToShowWithError: %@", [error localizedDescription]);
}
/// Tells the delegate that the interstitial ad opened.
- (void)yumiMediationInterstitialDidOpen:(YumiMediationInterstitial *)interstitial {
  NSLog(@"YumiMediationInterstitialDidOpen");
}
/// Tells the delegate that the interstitial ad start playing.
- (void)yumiMediationInterstitialDidStartPlaying:(YumiMediationInterstitial *)interstitial {
  NSLog(@"YumiMediationInterstitialDidStartPlaying");
}
/// Tells the delegate that the interstitial is to be animated off the screen.
- (void)yumiMediationInterstitialDidClosed:(YumiMediationInterstitial *)interstitial {
  NSLog(@"YumiMediationInterstitialDidClosed");
}
/// Tells the delegate that the interstitial ad has been clicked.
- (void)yumiMediationInterstitialDidClick:(YumiMediationInterstitial *)interstitial {
  NSLog(@"YumiMediationInterstitialDidClick");
}
```