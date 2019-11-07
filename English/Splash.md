# Splash
## Create and Load Yumi Splash Ads

To ensure splash impression, it is recommended to operate as the followings when App launching.
for example：in your `AppDelegate.m`  `application:didFinishLaunchingWithOptions:` 

```objective-c
#import <YumiMediationSDK/YumiMediationSplash.h>
@interface AppDelegate () <YumiMediationSplashAdDelegate>
@property (nonatomic) YumiMediationSplash *yumiSplash;
@end

@implementation AppDelegate    
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {  	
  self.yumiSplash = [[YumiMediationSplash alloc] initWithPlacementID:@"YOUR_PLACEMWNT_ID" 
                                                           channelID:@"YOUR_CHANNEL_ID" 
                                                           versionID:@"YOUR_VERSION_ID"];
  self.yumiSplash.delegate = self;  
  return YES;
}
@end
```

## Set Splash Fetch Time
```objective-c
/// Ad load timeout, default 3s.
/// The network Baidu does't support it
[self.yumiSplash setFetchTime:3]; 
```

## Set Launch Image
```objective-c
/// The background image when splash is loaded, preferably the image of the APP launch
[self.yumiSplash setLaunchImage:[UIImage imageNamed:@"YOUR_IMAGENAME"]];
```

## Set Splash Orientation (Only Admob Support)
```objective-c
/// splash orientation
[self.yumiSplash setInterfaceOrientation:UIInterfaceOrientationPortrait];
```
## Show Splash Full Screen
```objective-c
[self.yumiSplash loadAdAndShowInWindow:[UIApplication sharedApplication].keyWindow];
```

## Show Splash With Bottom Custom View
```objective-c
/// bottom view's height should not exceed 15% of the screen height.
UIView *bottomView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, [UIScreen mainScreen].bounds.size.width, [UIScreen mainScreen].bounds.size.height * 0.10)];
bottomView.backgroundColor = [UIColor redColor];
[self.yumiSplash loadAdAndShowInWindow:[UIApplication sharedApplication].keyWindow withBottomView:bottomView];
```

## Delegate Implementation
```objective-c
- (void)yumiMediationSplashAdSuccessToShow:(YumiMediationSplash *)splash {
  NSLog(@"yumiMediationSplashAdSuccessToShow.");
}
- (void)yumiMediationSplashAdFailToShow:(YumiMediationSplash *)splash withError:(NSError *)error {
  NSLog(@"yumiMediationSplashAdFailToShow: %@", error);
}
- (void)yumiMediationSplashAdDidClick:(YumiMediationSplash *)splash {
  NSLog(@"yumiMediationSplashAdDidClick.");
}
- (void)yumiMediationSplashAdDidClose:(YumiMediationSplash *)splash {
  NSLog(@"yumiMediationSplashAdDidClose.");
}
```