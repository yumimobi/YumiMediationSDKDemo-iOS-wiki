# Rewarded Video

## Auto Load Yumi Video Ads

```objective-c
#import <YumiMediationSDK/YumiMediationVideo.h>
@implementation ViewController
//  The rewarded video placement will auto cached.
- (void)viewDidLoad {
  [super viewDidLoad];
  [[YumiMediationVideo sharedInstance] loadAdWithPlacementID:@"Your PlacementID"
                                                   channelID:@"Your ChannelID"
                                                   versionID:@"Your VersionID"];
  [YumiMediationVideo sharedInstance].delegate = self;
}
@end
```
## Show Rewarded Video

```objective-c
- (IBAction)presentYumiMediationVideo:(id)sender {
  if ([[YumiMediationVideo sharedInstance] isReady]) {
    [[YumiMediationVideo sharedInstance] presentFromRootViewController:self];
  } else {
    NSLog(@"Ad wasn't ready");
  }
}
```

## Delegate Implementation

```objective-c
/// Tells the delegate that the video ad was received.
- (void)yumiMediationVideoDidReceiveAd:(YumiMediationVideo *)video {
  NSLog(@"YumiMediationVideoDidReceiveAd");
}
/// Tells the delegate that the video ad failed to load.
- (void)yumiMediationVideo:(YumiMediationVideo *)video didFailToLoadWithError:(NSError *)error {
  NSLog(@"YumiMediationVideoDidFailToLoadWithError:%@",[error localizedDescription]);
}
/// Tells the delegate that the video ad failed to show.
- (void)yumiMediationVideo:(YumiMediationVideo *)video didFailToShowWithError:(NSError *)error {
  NSLog(@"YumiMediationVideoDidFailToShowWithError:%@",[error localizedDescription]);
}
/// Tells the delegate that the video ad opened.
- (void)yumiMediationVideoDidOpen:(YumiMediationVideo *)video {
  NSLog(@"YumiMediationVideoDidOpen");
}
/// Tells the delegate that the video ad start playing.
- (void)yumiMediationVideoDidStartPlaying:(YumiMediationVideo *)video {
  NSLog(@"YumiMediationVideoDidStartPlaying");
}
/// Tells the delegate that the video ad closed.
/// You can learn about the reward info by examining the ‘isRewarded’ value.
- (void)yumiMediationVideoDidClosed:(YumiMediationVideo *)video isRewarded:(BOOL)isRewarded {
  NSLog(@"YumiMediationVideoDidClosedWithReward:%d",isRewarded);
}
/// Tells the delegate that the video ad has rewarded the user.
- (void)yumiMediationVideoDidReward:(YumiMediationVideo *)video {
  NSLog(@"YumiMediationVideoDidReward");
}
/// Tells the delegate that the reward video ad has been clicked by the person.
- (void)yumiMediationVideoDidClick:(YumiMediationVideo *)video {
  NSLog(@"YumiMediationVideoDidClick");
}
```
